---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 05/30/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e9f11f68aa138ab74a0ffbbd0e32067bc984606d
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415670"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="0ba59-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ba59-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0ba59-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ba59-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ba59-106">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="0ba59-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="0ba59-107">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0ba59-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="0ba59-108">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="0ba59-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="0ba59-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="0ba59-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ba59-110">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0ba59-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="0ba59-111">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="0ba59-111">Run the app.</span></span>
> * <span data-ttu-id="0ba59-112">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="0ba59-112">Examine the project files.</span></span>

<span data-ttu-id="0ba59-113">Alla fine di questa esercitazione si avrà un'app Web Razor Pages funzionante, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="0ba59-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="0ba59-115">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0ba59-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ba59-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ba59-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ba59-117">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="0ba59-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="0ba59-118">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0ba59-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="0ba59-119">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="0ba59-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0ba59-120">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="0ba59-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="0ba59-122">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="0ba59-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="0ba59-124">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="0ba59-124">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ba59-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ba59-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0ba59-127">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0ba59-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="0ba59-128">Passare alla directory (`cd`) che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="0ba59-128">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="0ba59-129">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0ba59-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="0ba59-130">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="0ba59-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="0ba59-131">Il comando `code` apre la cartella *RazorPagesMovie* nell'istanza corrente di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0ba59-131">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="0ba59-132">Quando l'icona a forma di fiamma di OmniSharp sulla barra di stato diventa verde, viene visualizzata una finestra di dialogo con la richiesta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="0ba59-132">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="0ba59-133">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="0ba59-133">Select **Yes**.</span></span>

  <span data-ttu-id="0ba59-134">Una directory *.vscode* che contiene i file *launch.json* e *tasks.json* viene aggiunta alla directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="0ba59-134">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ba59-135">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="0ba59-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0ba59-136">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0ba59-136">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="0ba59-137">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0ba59-137">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="0ba59-138">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="0ba59-138">Open the project</span></span>

<span data-ttu-id="0ba59-139">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0ba59-139">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="0ba59-140">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="0ba59-140">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ba59-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ba59-141">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ba59-142">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="0ba59-142">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="0ba59-143">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="0ba59-143">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="0ba59-144">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="0ba59-144">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ba59-145">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="0ba59-145">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="0ba59-146">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="0ba59-146">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0ba59-147">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="0ba59-147">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="0ba59-148">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="0ba59-148">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0ba59-149">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0ba59-149">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="0ba59-151">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="0ba59-151">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ba59-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ba59-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="0ba59-154">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="0ba59-154">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0ba59-155">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0ba59-155">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="0ba59-156">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="0ba59-156">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0ba59-157">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="0ba59-157">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="0ba59-158">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="0ba59-158">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="0ba59-159">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="0ba59-159">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0ba59-160">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0ba59-160">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="0ba59-162">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="0ba59-162">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0ba59-164">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="0ba59-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="0ba59-165">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="0ba59-165">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="0ba59-166">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="0ba59-166">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="0ba59-167">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="0ba59-167">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="0ba59-168">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0ba59-168">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="0ba59-170">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="0ba59-170">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="0ba59-172">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="0ba59-172">Examine the project files</span></span>

<span data-ttu-id="0ba59-173">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="0ba59-173">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="0ba59-174">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="0ba59-174">Pages folder</span></span>

<span data-ttu-id="0ba59-175">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="0ba59-175">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="0ba59-176">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="0ba59-176">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="0ba59-177">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="0ba59-177">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="0ba59-178">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="0ba59-178">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="0ba59-179">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="0ba59-179">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="0ba59-180">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="0ba59-180">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="0ba59-181">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="0ba59-181">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="0ba59-182">Per ulteriori informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="0ba59-182">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="0ba59-183">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="0ba59-183">wwwroot folder</span></span>

<span data-ttu-id="0ba59-184">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="0ba59-184">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="0ba59-185">Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="0ba59-185">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="0ba59-186">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="0ba59-186">appSettings.json</span></span>

<span data-ttu-id="0ba59-187">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="0ba59-187">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="0ba59-188">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0ba59-188">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="0ba59-189">Program.cs</span><span class="sxs-lookup"><span data-stu-id="0ba59-189">Program.cs</span></span>

<span data-ttu-id="0ba59-190">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="0ba59-190">Contains the entry point for the program.</span></span> <span data-ttu-id="0ba59-191">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="0ba59-191">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="0ba59-192">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="0ba59-192">Startup.cs</span></span>

<span data-ttu-id="0ba59-193">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="0ba59-193">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="0ba59-194">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="0ba59-194">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ba59-195">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0ba59-195">Additional resources</span></span>

* [<span data-ttu-id="0ba59-196">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="0ba59-196">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="0ba59-197">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0ba59-197">Next steps</span></span>

<span data-ttu-id="0ba59-198">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="0ba59-198">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ba59-199">Creazione di un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0ba59-199">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="0ba59-200">Esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="0ba59-200">Ran the app.</span></span>
> * <span data-ttu-id="0ba59-201">Esame dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="0ba59-201">Examined the project files.</span></span>

<span data-ttu-id="0ba59-202">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="0ba59-202">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0ba59-203">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="0ba59-203">Add a model</span></span>](xref:tutorials/razor-pages/model)
