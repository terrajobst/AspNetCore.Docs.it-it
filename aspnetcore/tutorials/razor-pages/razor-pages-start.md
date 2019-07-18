---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 06/03/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7e228c99b4d55c14cea9c915cf06a7fbbbd5af44
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855730"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e1e0c-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e0c-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e1e0c-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1e0c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e1e0c-106">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="e1e0c-107">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="e1e0c-108">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="e1e0c-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e1e0c-110">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="e1e0c-111">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-111">Run the app.</span></span>
> * <span data-ttu-id="e1e0c-112">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-112">Examine the project files.</span></span>

<span data-ttu-id="e1e0c-113">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="e1e0c-115">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e1e0c-115">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1e0c-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1e0c-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1e0c-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1e0c-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1e0c-118">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1e0c-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="e1e0c-119">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e1e0c-119">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1e0c-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1e0c-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1e0c-121">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="e1e0c-122">Creare una nuova applicazione Web ASP.NET Core e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-122">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="e1e0c-124">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-124">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e1e0c-125">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-125">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="e1e0c-127">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa **Applicazione Web** e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-127">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="e1e0c-129">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-129">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1e0c-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1e0c-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e1e0c-132">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e1e0c-132">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="e1e0c-133">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-133">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="e1e0c-134">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-134">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="e1e0c-135">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-135">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="e1e0c-136">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-136">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="e1e0c-137">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="e1e0c-137">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="e1e0c-138">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-138">Select **Yes**.</span></span>

  <span data-ttu-id="e1e0c-139">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-139">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1e0c-140">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1e0c-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e1e0c-141">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-141">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="e1e0c-142">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-142">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="e1e0c-143">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="e1e0c-143">Open the project</span></span>

<span data-ttu-id="e1e0c-144">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-144">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="e1e0c-145">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="e1e0c-145">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1e0c-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1e0c-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1e0c-147">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-147">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="e1e0c-148">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e1e0c-149">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-149">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e1e0c-150">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-150">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="e1e0c-151">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-151">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e1e0c-152">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="e1e0c-153">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-153">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e1e0c-154">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e1e0c-154">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="e1e0c-156">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-156">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1e0c-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1e0c-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="e1e0c-159">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="e1e0c-160">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="e1e0c-161">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e1e0c-162">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e1e0c-163">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-163">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="e1e0c-164">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-164">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e1e0c-165">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e1e0c-165">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="e1e0c-167">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-167">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1e0c-169">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1e0c-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="e1e0c-170">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-170">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="e1e0c-171">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-171">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="e1e0c-172">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-172">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="e1e0c-173">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e1e0c-173">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="e1e0c-175">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-175">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="e1e0c-177">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="e1e0c-177">Examine the project files</span></span>

<span data-ttu-id="e1e0c-178">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-178">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="e1e0c-179">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="e1e0c-179">Pages folder</span></span>

<span data-ttu-id="e1e0c-180">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-180">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="e1e0c-181">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-181">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="e1e0c-182">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-182">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="e1e0c-183">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-183">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="e1e0c-184">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-184">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="e1e0c-185">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-185">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="e1e0c-186">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-186">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="e1e0c-187">Per altre informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-187">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="e1e0c-188">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="e1e0c-188">wwwroot folder</span></span>

<span data-ttu-id="e1e0c-189">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-189">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="e1e0c-190">Per altre informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-190">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="e1e0c-191">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="e1e0c-191">appSettings.json</span></span>

<span data-ttu-id="e1e0c-192">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-192">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="e1e0c-193">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-193">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="e1e0c-194">Program.cs</span><span class="sxs-lookup"><span data-stu-id="e1e0c-194">Program.cs</span></span>

<span data-ttu-id="e1e0c-195">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-195">Contains the entry point for the program.</span></span> <span data-ttu-id="e1e0c-196">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-196">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="e1e0c-197">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="e1e0c-197">Startup.cs</span></span>

<span data-ttu-id="e1e0c-198">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-198">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="e1e0c-199">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-199">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1e0c-200">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e1e0c-200">Additional resources</span></span>

* [<span data-ttu-id="e1e0c-201">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="e1e0c-201">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="e1e0c-202">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e1e0c-202">Next steps</span></span>

<span data-ttu-id="e1e0c-203">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-203">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e1e0c-204">Creazione di un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-204">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="e1e0c-205">Esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-205">Ran the app.</span></span>
> * <span data-ttu-id="e1e0c-206">Esame dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="e1e0c-206">Examined the project files.</span></span>

<span data-ttu-id="e1e0c-207">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="e1e0c-207">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e1e0c-208">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="e1e0c-208">Add a model</span></span>](xref:tutorials/razor-pages/model)
