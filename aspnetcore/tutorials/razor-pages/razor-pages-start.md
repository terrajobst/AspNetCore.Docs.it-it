---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 0cc00cb85b6054752417b82c783cfd4c306aeda5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082568"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="79221-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79221-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="79221-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="79221-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="79221-106">Questa è la prima esercitazione di una serie che illustra i concetti di base per la creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="79221-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="79221-107">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="79221-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="79221-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="79221-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79221-109">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="79221-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="79221-110">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="79221-110">Run the app.</span></span>
> * <span data-ttu-id="79221-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="79221-111">Examine the project files.</span></span>

<span data-ttu-id="79221-112">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="79221-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="79221-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="79221-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79221-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79221-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79221-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79221-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79221-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="79221-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="79221-118">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="79221-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79221-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79221-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79221-120">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="79221-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="79221-121">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="79221-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="79221-122">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="79221-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="79221-123">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="79221-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="79221-124">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="79221-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="79221-125">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="79221-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="79221-126">Selezionare **ASP.NET Core 3.0** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="79221-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="79221-128">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="79221-128">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79221-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79221-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79221-131">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="79221-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="79221-132">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="79221-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="79221-133">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="79221-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="79221-134">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="79221-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="79221-135">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="79221-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="79221-136">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="79221-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="79221-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="79221-137">Select **Yes**.</span></span>

  <span data-ttu-id="79221-138">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="79221-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79221-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="79221-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79221-140">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="79221-140">Select **File** > **New Solution**.</span></span>

![Nuova soluzione macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="79221-142">Selezionare **.NET Core** > **App** > **Applicazione Web** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="79221-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="79221-144">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** impostare **Framework di destinazione** su **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="79221-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![Selezione di .NET Core 3.0 per macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="79221-146">Denominare il progetto **RazorPagesMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="79221-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="79221-148">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="79221-148">Open the project</span></span>

<span data-ttu-id="79221-149">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="79221-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="79221-150">Esecuzione dell'app</span><span class="sxs-lookup"><span data-stu-id="79221-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79221-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79221-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79221-152">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="79221-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="79221-153">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="79221-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="79221-154">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="79221-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="79221-155">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="79221-156">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="79221-157">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="79221-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79221-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79221-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="79221-159">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="79221-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="79221-160">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79221-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="79221-161">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="79221-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="79221-162">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="79221-163">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79221-164">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="79221-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="79221-165">Premere **ALT+CMD+INVIO** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="79221-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="79221-166">In alternativa, passare alla barra dei menu e andare a Esegui>Avvia senza eseguire debug.</span><span class="sxs-lookup"><span data-stu-id="79221-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="79221-167">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79221-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="79221-168">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="79221-168">Examine the project files</span></span>

<span data-ttu-id="79221-169">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="79221-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="79221-170">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="79221-170">Pages folder</span></span>

<span data-ttu-id="79221-171">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="79221-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="79221-172">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="79221-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="79221-173">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="79221-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="79221-174">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="79221-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="79221-175">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="79221-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="79221-176">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="79221-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="79221-177">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="79221-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="79221-178">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="79221-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="79221-179">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="79221-179">wwwroot folder</span></span>

<span data-ttu-id="79221-180">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="79221-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="79221-181">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="79221-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="79221-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="79221-182">appSettings.json</span></span>

<span data-ttu-id="79221-183">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="79221-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="79221-184">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="79221-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="79221-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="79221-185">Program.cs</span></span>

<span data-ttu-id="79221-186">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="79221-186">Contains the entry point for the program.</span></span> <span data-ttu-id="79221-187">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="79221-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="79221-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="79221-188">Startup.cs</span></span>

<span data-ttu-id="79221-189">Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="79221-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="79221-190">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="79221-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79221-191">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="79221-191">Next steps</span></span>

<span data-ttu-id="79221-192">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="79221-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="79221-193">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="79221-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="79221-194">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="79221-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="79221-195">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="79221-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="79221-196">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="79221-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="79221-197">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="79221-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79221-198">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="79221-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="79221-199">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="79221-199">Run the app.</span></span>
> * <span data-ttu-id="79221-200">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="79221-200">Examine the project files.</span></span>

<span data-ttu-id="79221-201">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="79221-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="79221-203">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="79221-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79221-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79221-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79221-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79221-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79221-206">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="79221-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="79221-207">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="79221-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79221-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79221-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79221-209">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="79221-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="79221-210">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="79221-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="79221-212">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="79221-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="79221-213">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="79221-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="79221-215">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="79221-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="79221-217">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="79221-217">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79221-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79221-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79221-220">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="79221-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="79221-221">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="79221-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="79221-222">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="79221-222">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="79221-223">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="79221-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="79221-224">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="79221-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="79221-225">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="79221-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="79221-226">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="79221-226">Select **Yes**.</span></span>

  <span data-ttu-id="79221-227">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="79221-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79221-228">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="79221-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79221-229">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="79221-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="79221-230">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="79221-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="79221-231">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="79221-231">Open the project</span></span>

<span data-ttu-id="79221-232">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="79221-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="79221-233">Esecuzione dell'app</span><span class="sxs-lookup"><span data-stu-id="79221-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79221-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79221-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79221-235">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="79221-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="79221-236">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="79221-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="79221-237">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="79221-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="79221-238">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="79221-239">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="79221-240">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="79221-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="79221-241">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="79221-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="79221-242">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="79221-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="79221-244">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="79221-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79221-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79221-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="79221-247">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="79221-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="79221-248">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79221-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="79221-249">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="79221-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="79221-250">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="79221-251">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="79221-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="79221-252">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="79221-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="79221-253">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="79221-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="79221-255">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="79221-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79221-257">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="79221-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="79221-258">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="79221-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="79221-259">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="79221-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="79221-260">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="79221-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="79221-261">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="79221-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="79221-263">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="79221-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="79221-265">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="79221-265">Examine the project files</span></span>

<span data-ttu-id="79221-266">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="79221-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="79221-267">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="79221-267">Pages folder</span></span>

<span data-ttu-id="79221-268">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="79221-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="79221-269">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="79221-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="79221-270">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="79221-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="79221-271">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="79221-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="79221-272">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="79221-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="79221-273">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="79221-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="79221-274">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="79221-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="79221-275">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="79221-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="79221-276">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="79221-276">wwwroot folder</span></span>

<span data-ttu-id="79221-277">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="79221-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="79221-278">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="79221-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="79221-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="79221-279">appSettings.json</span></span>

<span data-ttu-id="79221-280">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="79221-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="79221-281">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="79221-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="79221-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="79221-282">Program.cs</span></span>

<span data-ttu-id="79221-283">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="79221-283">Contains the entry point for the program.</span></span> <span data-ttu-id="79221-284">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="79221-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="79221-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="79221-285">Startup.cs</span></span>

<span data-ttu-id="79221-286">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="79221-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="79221-287">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="79221-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79221-288">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="79221-288">Additional resources</span></span>

* [<span data-ttu-id="79221-289">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="79221-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="79221-290">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="79221-290">Next steps</span></span>

<span data-ttu-id="79221-291">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="79221-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="79221-292">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="79221-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
