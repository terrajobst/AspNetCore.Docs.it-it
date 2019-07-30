---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371971"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="bdc41-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bdc41-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="bdc41-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bdc41-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="bdc41-106">Questa è la prima esercitazione di una serie che illustra i concetti di base per la creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdc41-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="bdc41-107">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="bdc41-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="bdc41-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="bdc41-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bdc41-109">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdc41-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="bdc41-110">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bdc41-110">Run the app.</span></span>
> * <span data-ttu-id="bdc41-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-111">Examine the project files.</span></span>

<span data-ttu-id="bdc41-112">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="bdc41-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="bdc41-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bdc41-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdc41-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdc41-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bdc41-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdc41-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bdc41-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bdc41-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="bdc41-118">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bdc41-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdc41-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdc41-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdc41-120">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bdc41-121">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="bdc41-122">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="bdc41-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="bdc41-123">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="bdc41-124">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="bdc41-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="bdc41-125">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="bdc41-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="bdc41-126">Selezionare **ASP.NET Core 3.0** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="bdc41-128">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="bdc41-128">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bdc41-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdc41-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bdc41-131">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="bdc41-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="bdc41-132">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="bdc41-133">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bdc41-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="bdc41-134">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="bdc41-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="bdc41-135">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bdc41-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="bdc41-136">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="bdc41-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="bdc41-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-137">Select **Yes**.</span></span>

  <span data-ttu-id="bdc41-138">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bdc41-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bdc41-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bdc41-140">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bdc41-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="bdc41-141">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdc41-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="bdc41-142">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="bdc41-142">Open the project</span></span>

<span data-ttu-id="bdc41-143">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bdc41-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="bdc41-144">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="bdc41-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdc41-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdc41-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdc41-146">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="bdc41-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="bdc41-147">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="bdc41-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="bdc41-148">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="bdc41-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bdc41-149">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="bdc41-150">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="bdc41-151">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bdc41-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdc41-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="bdc41-153">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="bdc41-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="bdc41-154">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="bdc41-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="bdc41-155">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="bdc41-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bdc41-156">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="bdc41-157">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bdc41-158">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bdc41-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="bdc41-159">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="bdc41-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="bdc41-160">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="bdc41-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="bdc41-161">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="bdc41-161">Examine the project files</span></span>

<span data-ttu-id="bdc41-162">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="bdc41-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="bdc41-163">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="bdc41-163">Pages folder</span></span>

<span data-ttu-id="bdc41-164">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="bdc41-165">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="bdc41-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="bdc41-166">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="bdc41-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="bdc41-167">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="bdc41-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="bdc41-168">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="bdc41-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="bdc41-169">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="bdc41-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="bdc41-170">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="bdc41-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="bdc41-171">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="bdc41-172">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="bdc41-172">wwwroot folder</span></span>

<span data-ttu-id="bdc41-173">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="bdc41-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="bdc41-174">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="bdc41-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="bdc41-175">appSettings.json</span></span>

<span data-ttu-id="bdc41-176">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="bdc41-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="bdc41-177">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="bdc41-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="bdc41-178">Program.cs</span></span>

<span data-ttu-id="bdc41-179">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="bdc41-179">Contains the entry point for the program.</span></span> <span data-ttu-id="bdc41-180">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="bdc41-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="bdc41-181">Startup.cs</span></span>

<span data-ttu-id="bdc41-182">Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="bdc41-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="bdc41-183">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdc41-184">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bdc41-184">Next steps</span></span>

<span data-ttu-id="bdc41-185">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="bdc41-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bdc41-186">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="bdc41-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bdc41-187">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="bdc41-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="bdc41-188">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdc41-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="bdc41-189">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="bdc41-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="bdc41-190">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="bdc41-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bdc41-191">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdc41-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="bdc41-192">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bdc41-192">Run the app.</span></span>
> * <span data-ttu-id="bdc41-193">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-193">Examine the project files.</span></span>

<span data-ttu-id="bdc41-194">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="bdc41-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="bdc41-196">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bdc41-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdc41-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdc41-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bdc41-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdc41-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bdc41-199">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bdc41-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="bdc41-200">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bdc41-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdc41-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdc41-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdc41-202">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="bdc41-203">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="bdc41-205">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="bdc41-206">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="bdc41-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="bdc41-208">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="bdc41-210">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="bdc41-210">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bdc41-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdc41-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bdc41-213">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="bdc41-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="bdc41-214">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="bdc41-215">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bdc41-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="bdc41-216">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="bdc41-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="bdc41-217">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bdc41-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="bdc41-218">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="bdc41-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="bdc41-219">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="bdc41-219">Select **Yes**.</span></span>

  <span data-ttu-id="bdc41-220">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bdc41-221">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bdc41-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bdc41-222">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bdc41-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="bdc41-223">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdc41-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="bdc41-224">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="bdc41-224">Open the project</span></span>

<span data-ttu-id="bdc41-225">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bdc41-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="bdc41-226">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="bdc41-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bdc41-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdc41-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdc41-228">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="bdc41-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="bdc41-229">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="bdc41-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="bdc41-230">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="bdc41-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bdc41-231">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="bdc41-232">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="bdc41-233">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="bdc41-234">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="bdc41-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="bdc41-235">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="bdc41-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="bdc41-237">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="bdc41-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bdc41-239">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdc41-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="bdc41-240">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="bdc41-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="bdc41-241">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="bdc41-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="bdc41-242">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="bdc41-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="bdc41-243">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="bdc41-244">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="bdc41-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="bdc41-245">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="bdc41-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="bdc41-246">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="bdc41-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="bdc41-248">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="bdc41-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bdc41-250">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bdc41-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="bdc41-251">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="bdc41-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="bdc41-252">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="bdc41-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="bdc41-253">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="bdc41-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="bdc41-254">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="bdc41-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="bdc41-256">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="bdc41-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="bdc41-258">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="bdc41-258">Examine the project files</span></span>

<span data-ttu-id="bdc41-259">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="bdc41-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="bdc41-260">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="bdc41-260">Pages folder</span></span>

<span data-ttu-id="bdc41-261">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="bdc41-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="bdc41-262">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="bdc41-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="bdc41-263">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="bdc41-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="bdc41-264">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="bdc41-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="bdc41-265">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="bdc41-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="bdc41-266">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="bdc41-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="bdc41-267">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="bdc41-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="bdc41-268">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="bdc41-269">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="bdc41-269">wwwroot folder</span></span>

<span data-ttu-id="bdc41-270">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="bdc41-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="bdc41-271">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="bdc41-272">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="bdc41-272">appSettings.json</span></span>

<span data-ttu-id="bdc41-273">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="bdc41-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="bdc41-274">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="bdc41-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="bdc41-275">Program.cs</span></span>

<span data-ttu-id="bdc41-276">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="bdc41-276">Contains the entry point for the program.</span></span> <span data-ttu-id="bdc41-277">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="bdc41-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="bdc41-278">Startup.cs</span></span>

<span data-ttu-id="bdc41-279">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="bdc41-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="bdc41-280">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="bdc41-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bdc41-281">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bdc41-281">Additional resources</span></span>

* [<span data-ttu-id="bdc41-282">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bdc41-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="bdc41-283">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bdc41-283">Next steps</span></span>

<span data-ttu-id="bdc41-284">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="bdc41-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bdc41-285">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="bdc41-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end