---
title: "Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 6e1d58ccd83f7d7c1083dc2bf9ce7476650812a1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658543"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="6b7b0-104">Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b7b0-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="6b7b0-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6b7b0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="6b7b0-106">Questa è la prima esercitazione di una serie che illustra i concetti di base per la creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="6b7b0-107">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="6b7b0-108">In questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b7b0-109">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="6b7b0-110">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-110">Run the app.</span></span>
> * <span data-ttu-id="6b7b0-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-111">Examine the project files.</span></span>

<span data-ttu-id="6b7b0-112">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="6b7b0-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="6b7b0-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6b7b0-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b7b0-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="6b7b0-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b7b0-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6b7b0-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b7b0-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="6b7b0-118">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b7b0-118">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6b7b0-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b7b0-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6b7b0-120">Scegliere **nuovo** > **progetto**dal menu **file** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6b7b0-121">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="6b7b0-122">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="6b7b0-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="6b7b0-123">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="6b7b0-124">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="6b7b0-125">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="6b7b0-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="6b7b0-126">Selezionare **ASP.NET Core 3,1** nell'elenco a discesa, **applicazione Web**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="6b7b0-128">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-128">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="6b7b0-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b7b0-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6b7b0-131">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6b7b0-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="6b7b0-132">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="6b7b0-133">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="6b7b0-134">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="6b7b0-135">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="6b7b0-136">Dopo che l'icona di OmniSharp Flame della barra di stato è verde, una finestra di dialogo richiede che le **risorse necessarie per la compilazione e il debug siano mancanti da' RazorPagesMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="6b7b0-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="6b7b0-137">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-137">Select **Yes**.</span></span>

  <span data-ttu-id="6b7b0-138">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6b7b0-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b7b0-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6b7b0-140">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-140">Select **File** > **New Solution**.</span></span>

![Nuova soluzione macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="6b7b0-142">Selezionare l' **applicazione Web** **.NET Core** > **app** > > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="6b7b0-144">Nella finestra di dialogo **Configura nuova applicazione Web** impostare il **Framework di destinazione** su **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-144">In the **Configure your new Web Application** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![selezione di macOS .NET Core 3,1](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="6b7b0-146">Denominare il progetto **RazorPagesMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="6b7b0-148">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="6b7b0-148">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="6b7b0-149">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="6b7b0-149">Examine the project files</span></span>

<span data-ttu-id="6b7b0-150">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-150">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="6b7b0-151">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="6b7b0-151">Pages folder</span></span>

<span data-ttu-id="6b7b0-152">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-152">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="6b7b0-153">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-153">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="6b7b0-154">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-154">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="6b7b0-155">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-155">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="6b7b0-156">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-156">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="6b7b0-157">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-157">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="6b7b0-158">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-158">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="6b7b0-159">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-159">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="6b7b0-160">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="6b7b0-160">wwwroot folder</span></span>

<span data-ttu-id="6b7b0-161">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-161">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="6b7b0-162">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-162">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="6b7b0-163">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="6b7b0-163">appSettings.json</span></span>

<span data-ttu-id="6b7b0-164">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-164">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="6b7b0-165">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-165">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="6b7b0-166">Program.cs</span><span class="sxs-lookup"><span data-stu-id="6b7b0-166">Program.cs</span></span>

<span data-ttu-id="6b7b0-167">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-167">Contains the entry point for the program.</span></span> <span data-ttu-id="6b7b0-168">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="6b7b0-169">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="6b7b0-169">Startup.cs</span></span>

<span data-ttu-id="6b7b0-170">Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-170">Contains code that configures app behavior.</span></span> <span data-ttu-id="6b7b0-171">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-171">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b7b0-172">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6b7b0-172">Next steps</span></span>

<span data-ttu-id="6b7b0-173">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-173">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6b7b0-174">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="6b7b0-174">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6b7b0-175">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-175">This is the first tutorial of a series.</span></span> <span data-ttu-id="6b7b0-176">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-176">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="6b7b0-177">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-177">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="6b7b0-178">In questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-178">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b7b0-179">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-179">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="6b7b0-180">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-180">Run the app.</span></span>
> * <span data-ttu-id="6b7b0-181">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-181">Examine the project files.</span></span>

<span data-ttu-id="6b7b0-182">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-182">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="6b7b0-184">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="6b7b0-184">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6b7b0-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b7b0-185">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="6b7b0-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b7b0-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6b7b0-187">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b7b0-187">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="6b7b0-188">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b7b0-188">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6b7b0-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b7b0-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6b7b0-190">Scegliere **nuovo** > **progetto**dal menu **file** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="6b7b0-191">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-191">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="6b7b0-193">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-193">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="6b7b0-194">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-194">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="6b7b0-196">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-196">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="6b7b0-198">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-198">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="6b7b0-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b7b0-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6b7b0-201">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6b7b0-201">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="6b7b0-202">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-202">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="6b7b0-203">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-203">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="6b7b0-204">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-204">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="6b7b0-205">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-205">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="6b7b0-206">Dopo che l'icona di OmniSharp Flame della barra di stato è verde, una finestra di dialogo richiede che le **risorse necessarie per la compilazione e il debug siano mancanti da' RazorPagesMovie '. Aggiungerli?**</span><span class="sxs-lookup"><span data-stu-id="6b7b0-206">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="6b7b0-207">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-207">Select **Yes**.</span></span>

  <span data-ttu-id="6b7b0-208">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-208">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6b7b0-209">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b7b0-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6b7b0-210">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-210">Select **File** > **New Solution**.</span></span>

![Nuova soluzione macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="6b7b0-212">Selezionare l' **applicazione Web** **.NET Core** > **app** > > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-212">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="6b7b0-214">Nella finestra di dialogo **configurare la nuova ASP.NET Core API Web** impostare il **Framework di destinazione** su **.NET Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-214">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Selezione di .NET Core 3.0 per macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="6b7b0-216">Denominare il progetto **RazorPagesMovie**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-216">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="6b7b0-218">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="6b7b0-218">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6b7b0-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b7b0-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6b7b0-220">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-220">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="6b7b0-221">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-221">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="6b7b0-222">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-222">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6b7b0-223">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-223">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="6b7b0-224">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-224">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="6b7b0-225">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-225">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="6b7b0-226">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-226">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="6b7b0-227">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="6b7b0-227">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="6b7b0-229">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-229">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="6b7b0-231">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b7b0-231">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="6b7b0-232">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-232">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="6b7b0-233">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-233">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="6b7b0-234">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-234">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6b7b0-235">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-235">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="6b7b0-236">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-236">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="6b7b0-237">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-237">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="6b7b0-238">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="6b7b0-238">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="6b7b0-240">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-240">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="6b7b0-242">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b7b0-242">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="6b7b0-243">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-243">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="6b7b0-244">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-244">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="6b7b0-245">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="6b7b0-246">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="6b7b0-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="6b7b0-248">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="6b7b0-250">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="6b7b0-250">Examine the project files</span></span>

<span data-ttu-id="6b7b0-251">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-251">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="6b7b0-252">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="6b7b0-252">Pages folder</span></span>

<span data-ttu-id="6b7b0-253">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-253">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="6b7b0-254">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-254">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="6b7b0-255">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-255">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="6b7b0-256">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-256">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="6b7b0-257">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-257">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="6b7b0-258">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-258">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="6b7b0-259">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-259">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="6b7b0-260">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-260">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="6b7b0-261">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="6b7b0-261">wwwroot folder</span></span>

<span data-ttu-id="6b7b0-262">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-262">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="6b7b0-263">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-263">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="6b7b0-264">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="6b7b0-264">appSettings.json</span></span>

<span data-ttu-id="6b7b0-265">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-265">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="6b7b0-266">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-266">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="6b7b0-267">Program.cs</span><span class="sxs-lookup"><span data-stu-id="6b7b0-267">Program.cs</span></span>

<span data-ttu-id="6b7b0-268">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-268">Contains the entry point for the program.</span></span> <span data-ttu-id="6b7b0-269">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-269">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="6b7b0-270">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="6b7b0-270">Startup.cs</span></span>

<span data-ttu-id="6b7b0-271">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-271">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="6b7b0-272">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="6b7b0-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6b7b0-273">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6b7b0-273">Additional resources</span></span>

* [<span data-ttu-id="6b7b0-274">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6b7b0-274">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="6b7b0-275">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6b7b0-275">Next steps</span></span>

<span data-ttu-id="6b7b0-276">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="6b7b0-276">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6b7b0-277">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="6b7b0-277">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
