---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682799"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="9f6f9-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f6f9-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="9f6f9-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9f6f9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="9f6f9-106">Questa è la prima esercitazione di una serie che illustra i concetti di base per la creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9f6f9-107">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9f6f9-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f6f9-109">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9f6f9-110">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-110">Run the app.</span></span>
> * <span data-ttu-id="9f6f9-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-111">Examine the project files.</span></span>

<span data-ttu-id="9f6f9-112">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9f6f9-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9f6f9-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f6f9-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6f9-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f6f9-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f6f9-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f6f9-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f6f9-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9f6f9-118">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9f6f9-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f6f9-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6f9-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f6f9-120">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9f6f9-121">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="9f6f9-122">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="9f6f9-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="9f6f9-123">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9f6f9-124">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="9f6f9-125">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="9f6f9-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="9f6f9-126">Selezionare **ASP.NET Core 3.0** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="9f6f9-128">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-128">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f6f9-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f6f9-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9f6f9-131">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9f6f9-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9f6f9-132">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9f6f9-133">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9f6f9-134">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9f6f9-135">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9f6f9-136">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="9f6f9-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9f6f9-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-137">Select **Yes**.</span></span>

  <span data-ttu-id="9f6f9-138">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f6f9-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f6f9-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9f6f9-140">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9f6f9-141">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9f6f9-142">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="9f6f9-142">Open the project</span></span>

<span data-ttu-id="9f6f9-143">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9f6f9-144">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="9f6f9-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f6f9-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6f9-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f6f9-146">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9f6f9-147">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9f6f9-148">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9f6f9-149">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9f6f9-150">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9f6f9-151">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f6f9-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f6f9-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9f6f9-153">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9f6f9-154">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9f6f9-155">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9f6f9-156">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9f6f9-157">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f6f9-158">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f6f9-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9f6f9-159">Premere **ALT+CMD+INVIO** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="9f6f9-160">In alternativa, passare alla barra dei menu e andare a Esegui>Avvia senza eseguire debug.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="9f6f9-161">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9f6f9-162">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="9f6f9-162">Examine the project files</span></span>

<span data-ttu-id="9f6f9-163">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9f6f9-164">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="9f6f9-164">Pages folder</span></span>

<span data-ttu-id="9f6f9-165">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9f6f9-166">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9f6f9-167">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9f6f9-168">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9f6f9-169">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9f6f9-170">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9f6f9-171">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9f6f9-172">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9f6f9-173">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="9f6f9-173">wwwroot folder</span></span>

<span data-ttu-id="9f6f9-174">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9f6f9-175">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9f6f9-176">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9f6f9-176">appSettings.json</span></span>

<span data-ttu-id="9f6f9-177">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9f6f9-178">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9f6f9-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9f6f9-179">Program.cs</span></span>

<span data-ttu-id="9f6f9-180">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-180">Contains the entry point for the program.</span></span> <span data-ttu-id="9f6f9-181">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9f6f9-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9f6f9-182">Startup.cs</span></span>

<span data-ttu-id="9f6f9-183">Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="9f6f9-184">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f6f9-185">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9f6f9-185">Next steps</span></span>

<span data-ttu-id="9f6f9-186">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9f6f9-187">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="9f6f9-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9f6f9-188">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="9f6f9-189">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9f6f9-190">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9f6f9-191">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f6f9-192">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9f6f9-193">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-193">Run the app.</span></span>
> * <span data-ttu-id="9f6f9-194">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-194">Examine the project files.</span></span>

<span data-ttu-id="9f6f9-195">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9f6f9-197">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9f6f9-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f6f9-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6f9-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f6f9-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f6f9-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f6f9-200">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f6f9-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9f6f9-201">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9f6f9-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f6f9-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6f9-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f6f9-203">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="9f6f9-204">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="9f6f9-206">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9f6f9-207">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="9f6f9-209">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="9f6f9-211">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-211">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f6f9-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f6f9-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9f6f9-214">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9f6f9-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9f6f9-215">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9f6f9-216">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9f6f9-217">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9f6f9-218">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9f6f9-219">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="9f6f9-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9f6f9-220">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-220">Select **Yes**.</span></span>

  <span data-ttu-id="9f6f9-221">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f6f9-222">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f6f9-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9f6f9-223">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9f6f9-224">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9f6f9-225">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="9f6f9-225">Open the project</span></span>

<span data-ttu-id="9f6f9-226">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9f6f9-227">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="9f6f9-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f6f9-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6f9-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f6f9-229">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9f6f9-230">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9f6f9-231">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9f6f9-232">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9f6f9-233">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9f6f9-234">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="9f6f9-235">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9f6f9-236">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9f6f9-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9f6f9-238">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f6f9-240">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f6f9-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9f6f9-241">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9f6f9-242">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9f6f9-243">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9f6f9-244">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9f6f9-245">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="9f6f9-246">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9f6f9-247">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9f6f9-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9f6f9-249">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f6f9-251">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f6f9-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9f6f9-252">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9f6f9-253">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="9f6f9-254">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9f6f9-255">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="9f6f9-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="9f6f9-257">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9f6f9-259">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="9f6f9-259">Examine the project files</span></span>

<span data-ttu-id="9f6f9-260">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9f6f9-261">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="9f6f9-261">Pages folder</span></span>

<span data-ttu-id="9f6f9-262">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9f6f9-263">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9f6f9-264">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9f6f9-265">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9f6f9-266">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9f6f9-267">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9f6f9-268">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9f6f9-269">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9f6f9-270">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="9f6f9-270">wwwroot folder</span></span>

<span data-ttu-id="9f6f9-271">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9f6f9-272">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9f6f9-273">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9f6f9-273">appSettings.json</span></span>

<span data-ttu-id="9f6f9-274">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9f6f9-275">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9f6f9-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9f6f9-276">Program.cs</span></span>

<span data-ttu-id="9f6f9-277">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-277">Contains the entry point for the program.</span></span> <span data-ttu-id="9f6f9-278">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9f6f9-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9f6f9-279">Startup.cs</span></span>

<span data-ttu-id="9f6f9-280">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="9f6f9-281">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9f6f9-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f6f9-282">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9f6f9-282">Additional resources</span></span>

* [<span data-ttu-id="9f6f9-283">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="9f6f9-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="9f6f9-284">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9f6f9-284">Next steps</span></span>

<span data-ttu-id="9f6f9-285">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="9f6f9-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9f6f9-286">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="9f6f9-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
