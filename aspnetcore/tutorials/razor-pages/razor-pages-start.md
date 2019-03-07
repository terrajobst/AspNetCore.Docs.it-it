---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 29d9369cfa6a4c76f015b5a819a27dfa280d4075
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346411"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="dc42a-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc42a-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="dc42a-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc42a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc42a-106">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="dc42a-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="dc42a-107">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dc42a-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="dc42a-108">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="dc42a-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="dc42a-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc42a-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dc42a-110">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dc42a-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="dc42a-111">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="dc42a-111">Run the app.</span></span>
> * <span data-ttu-id="dc42a-112">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="dc42a-112">Examine the project files.</span></span>

<span data-ttu-id="dc42a-113">Alla fine di questa esercitazione si avrà un'app Web Razor Pages, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="dc42a-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="dc42a-115">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="dc42a-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc42a-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc42a-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dc42a-117">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="dc42a-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="dc42a-118">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc42a-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="dc42a-119">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="dc42a-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="dc42a-120">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="dc42a-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="dc42a-122">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="dc42a-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="dc42a-124">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="dc42a-124">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc42a-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc42a-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dc42a-127">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dc42a-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="dc42a-128">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc42a-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="dc42a-129">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc42a-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="dc42a-130">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="dc42a-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="dc42a-131">Il comando `code` visualizza la cartella *RazorPagesMovie* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dc42a-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="dc42a-132">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="dc42a-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="dc42a-133">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="dc42a-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dc42a-134">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="dc42a-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc42a-135">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc42a-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="dc42a-136">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dc42a-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="dc42a-137">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="dc42a-137">Open the project</span></span>

<span data-ttu-id="dc42a-138">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="dc42a-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="dc42a-139">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="dc42a-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc42a-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc42a-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dc42a-141">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="dc42a-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="dc42a-142">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="dc42a-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="dc42a-143">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="dc42a-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dc42a-144">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="dc42a-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="dc42a-145">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="dc42a-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="dc42a-146">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="dc42a-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc42a-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc42a-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dc42a-148">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="dc42a-148">Press **Ctrl-F5** to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="dc42a-149">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dc42a-149">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="dc42a-150">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="dc42a-150">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="dc42a-151">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="dc42a-151">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="dc42a-152">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="dc42a-152">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dc42a-153">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="dc42a-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dc42a-154">Per avviare l'app, selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="dc42a-154">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="dc42a-155">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="dc42a-155">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* <span data-ttu-id="dc42a-156">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="dc42a-156">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="dc42a-157">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dc42a-157">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="dc42a-159">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="dc42a-159">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="dc42a-161">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="dc42a-161">Examine the project files</span></span>

<span data-ttu-id="dc42a-162">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="dc42a-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="dc42a-163">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="dc42a-163">Pages folder</span></span>

<span data-ttu-id="dc42a-164">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="dc42a-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="dc42a-165">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="dc42a-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="dc42a-166">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="dc42a-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="dc42a-167">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="dc42a-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="dc42a-168">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="dc42a-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="dc42a-169">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="dc42a-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="dc42a-170">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="dc42a-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="dc42a-171">Per ulteriori informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="dc42a-171">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="dc42a-172">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="dc42a-172">wwwroot folder</span></span>

<span data-ttu-id="dc42a-173">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="dc42a-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="dc42a-174">Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="dc42a-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="dc42a-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="dc42a-175">appSettings.json</span></span>

<span data-ttu-id="dc42a-176">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="dc42a-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="dc42a-177">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="dc42a-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="dc42a-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="dc42a-178">Program.cs</span></span>

<span data-ttu-id="dc42a-179">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="dc42a-179">Contains the entry point for the program.</span></span> <span data-ttu-id="dc42a-180">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="dc42a-180">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="dc42a-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="dc42a-181">Startup.cs</span></span>

<span data-ttu-id="dc42a-182">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="dc42a-182">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="dc42a-183">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="dc42a-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc42a-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dc42a-184">Additional resources</span></span>

* [<span data-ttu-id="dc42a-185">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="dc42a-185">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="dc42a-186">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dc42a-186">Next steps</span></span>

<span data-ttu-id="dc42a-187">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc42a-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dc42a-188">Creazione di un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dc42a-188">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="dc42a-189">Esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dc42a-189">Ran the app.</span></span>
> * <span data-ttu-id="dc42a-190">Esame dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="dc42a-190">Examined the project files.</span></span>

<span data-ttu-id="dc42a-191">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="dc42a-191">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dc42a-192">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="dc42a-192">Add a model</span></span>](xref:tutorials/razor-pages/model)
