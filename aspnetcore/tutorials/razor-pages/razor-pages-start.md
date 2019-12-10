---
title: "Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: b651437b698d01310f90c5f14832616c1896e6c0
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959099"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="08df7-104">Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08df7-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="08df7-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="08df7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="08df7-106">Questa è la prima esercitazione di una serie che illustra i concetti di base per la creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08df7-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="08df7-107">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="08df7-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="08df7-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="08df7-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08df7-109">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08df7-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="08df7-110">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="08df7-110">Run the app.</span></span>
> * <span data-ttu-id="08df7-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="08df7-111">Examine the project files.</span></span>

<span data-ttu-id="08df7-112">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="08df7-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="08df7-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="08df7-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08df7-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08df7-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08df7-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08df7-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08df7-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08df7-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="08df7-118">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08df7-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08df7-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08df7-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08df7-120">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="08df7-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="08df7-121">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="08df7-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="08df7-122">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="08df7-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="08df7-123">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="08df7-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="08df7-124">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="08df7-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="08df7-125">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="08df7-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="08df7-126">Selezionare **ASP.NET Core 3,1** nell'elenco a discesa, **applicazione Web**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="08df7-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="08df7-128">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="08df7-128">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08df7-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08df7-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08df7-131">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="08df7-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="08df7-132">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="08df7-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="08df7-133">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="08df7-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="08df7-134">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="08df7-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="08df7-135">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="08df7-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="08df7-136">Dopo che l'icona di OmniSharp Flame della barra di stato è verde, una finestra di dialogo richiede che le **risorse necessarie per la compilazione e il debug siano mancanti da' RazorPagesMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="08df7-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="08df7-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="08df7-137">Select **Yes**.</span></span>

  <span data-ttu-id="08df7-138">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="08df7-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08df7-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08df7-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="08df7-140">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="08df7-140">Select **File** > **New Solution**.</span></span>

![Nuova soluzione macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="08df7-142">Selezionare **.NET Core** > **App** > **Applicazione Web** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="08df7-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="08df7-144">Nella finestra di dialogo **configurare la nuova ASP.NET Core API Web** impostare il **Framework di destinazione** su **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="08df7-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Selezione di .NET Core 3.0 per macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="08df7-146">Denominare il progetto **RazorPagesMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="08df7-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="08df7-148">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="08df7-148">Open the project</span></span>

<span data-ttu-id="08df7-149">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="08df7-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="08df7-150">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="08df7-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="08df7-151">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="08df7-151">Examine the project files</span></span>

<span data-ttu-id="08df7-152">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="08df7-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="08df7-153">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="08df7-153">Pages folder</span></span>

<span data-ttu-id="08df7-154">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="08df7-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="08df7-155">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="08df7-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="08df7-156">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="08df7-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="08df7-157">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="08df7-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="08df7-158">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="08df7-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="08df7-159">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="08df7-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="08df7-160">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="08df7-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="08df7-161">Per ulteriori informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="08df7-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="08df7-162">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="08df7-162">wwwroot folder</span></span>

<span data-ttu-id="08df7-163">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="08df7-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="08df7-164">Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="08df7-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="08df7-165">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="08df7-165">appSettings.json</span></span>

<span data-ttu-id="08df7-166">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="08df7-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="08df7-167">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="08df7-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="08df7-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="08df7-168">Program.cs</span></span>

<span data-ttu-id="08df7-169">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="08df7-169">Contains the entry point for the program.</span></span> <span data-ttu-id="08df7-170">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="08df7-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="08df7-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="08df7-171">Startup.cs</span></span>

<span data-ttu-id="08df7-172">Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="08df7-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="08df7-173">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="08df7-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08df7-174">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="08df7-174">Next steps</span></span>

<span data-ttu-id="08df7-175">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="08df7-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="08df7-176">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="08df7-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="08df7-177">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="08df7-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="08df7-178">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08df7-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="08df7-179">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="08df7-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="08df7-180">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="08df7-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08df7-181">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08df7-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="08df7-182">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="08df7-182">Run the app.</span></span>
> * <span data-ttu-id="08df7-183">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="08df7-183">Examine the project files.</span></span>

<span data-ttu-id="08df7-184">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="08df7-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="08df7-186">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="08df7-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08df7-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08df7-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08df7-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08df7-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08df7-189">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08df7-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="08df7-190">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="08df7-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08df7-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08df7-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08df7-192">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="08df7-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="08df7-193">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="08df7-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="08df7-195">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="08df7-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="08df7-196">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="08df7-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="08df7-198">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="08df7-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="08df7-200">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="08df7-200">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08df7-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08df7-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08df7-203">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="08df7-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="08df7-204">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="08df7-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="08df7-205">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="08df7-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="08df7-206">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="08df7-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="08df7-207">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="08df7-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="08df7-208">Dopo che l'icona di OmniSharp Flame della barra di stato è verde, una finestra di dialogo richiede che le **risorse necessarie per la compilazione e il debug siano mancanti da' RazorPagesMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="08df7-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="08df7-209">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="08df7-209">Select **Yes**.</span></span>

  <span data-ttu-id="08df7-210">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="08df7-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08df7-211">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08df7-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="08df7-212">Dal Terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="08df7-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="08df7-213">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08df7-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="08df7-214">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="08df7-214">Open the project</span></span>

<span data-ttu-id="08df7-215">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="08df7-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="08df7-216">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="08df7-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08df7-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08df7-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08df7-218">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="08df7-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="08df7-219">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="08df7-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="08df7-220">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="08df7-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="08df7-221">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="08df7-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="08df7-222">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="08df7-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="08df7-223">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="08df7-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="08df7-224">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="08df7-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="08df7-225">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="08df7-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="08df7-227">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="08df7-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08df7-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08df7-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="08df7-230">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="08df7-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="08df7-231">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="08df7-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="08df7-232">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="08df7-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="08df7-233">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="08df7-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="08df7-234">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="08df7-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="08df7-235">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="08df7-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="08df7-236">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="08df7-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="08df7-238">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="08df7-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08df7-240">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08df7-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="08df7-241">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="08df7-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="08df7-242">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="08df7-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="08df7-243">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="08df7-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="08df7-244">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="08df7-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="08df7-246">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="08df7-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="08df7-248">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="08df7-248">Examine the project files</span></span>

<span data-ttu-id="08df7-249">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="08df7-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="08df7-250">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="08df7-250">Pages folder</span></span>

<span data-ttu-id="08df7-251">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="08df7-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="08df7-252">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="08df7-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="08df7-253">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="08df7-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="08df7-254">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="08df7-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="08df7-255">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="08df7-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="08df7-256">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="08df7-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="08df7-257">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="08df7-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="08df7-258">Per ulteriori informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="08df7-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="08df7-259">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="08df7-259">wwwroot folder</span></span>

<span data-ttu-id="08df7-260">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="08df7-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="08df7-261">Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="08df7-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="08df7-262">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="08df7-262">appSettings.json</span></span>

<span data-ttu-id="08df7-263">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="08df7-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="08df7-264">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="08df7-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="08df7-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="08df7-265">Program.cs</span></span>

<span data-ttu-id="08df7-266">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="08df7-266">Contains the entry point for the program.</span></span> <span data-ttu-id="08df7-267">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="08df7-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="08df7-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="08df7-268">Startup.cs</span></span>

<span data-ttu-id="08df7-269">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="08df7-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="08df7-270">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="08df7-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08df7-271">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="08df7-271">Additional resources</span></span>

* [<span data-ttu-id="08df7-272">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="08df7-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="08df7-273">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="08df7-273">Next steps</span></span>

<span data-ttu-id="08df7-274">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="08df7-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="08df7-275">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="08df7-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
