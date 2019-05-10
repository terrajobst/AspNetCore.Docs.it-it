---
title: "Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core"
author: rick-anderson
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d264ca4a605d8291e273a8f054c92e7eefa5548
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891156"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="21a9e-104">Esercitazione: Introduzione all'uso di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21a9e-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="21a9e-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21a9e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="21a9e-106">Questa è la prima esercitazione di una serie.</span><span class="sxs-lookup"><span data-stu-id="21a9e-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="21a9e-107">[La serie](xref:tutorials/razor-pages/index) illustra le nozioni di base della creazione di un'app Web ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="21a9e-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="21a9e-108">Al termine della serie si otterrà un'app che gestisce un database di film.</span><span class="sxs-lookup"><span data-stu-id="21a9e-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="21a9e-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="21a9e-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21a9e-110">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="21a9e-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="21a9e-111">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="21a9e-111">Run the app.</span></span>
> * <span data-ttu-id="21a9e-112">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="21a9e-112">Examine the project files.</span></span>

<span data-ttu-id="21a9e-113">Alla fine di questa esercitazione si avrà un'app Web Razor Pages, che verrà compilata in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="21a9e-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="21a9e-115">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="21a9e-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21a9e-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21a9e-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="21a9e-117">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="21a9e-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="21a9e-118">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="21a9e-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="21a9e-119">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="21a9e-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="21a9e-120">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="21a9e-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="21a9e-122">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="21a9e-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="21a9e-124">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="21a9e-124">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="21a9e-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21a9e-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="21a9e-127">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="21a9e-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="21a9e-128">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="21a9e-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="21a9e-129">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="21a9e-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="21a9e-130">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="21a9e-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="21a9e-131">Il comando `code` visualizza la cartella *RazorPagesMovie* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="21a9e-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="21a9e-132">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="21a9e-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="21a9e-133">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="21a9e-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="21a9e-134">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="21a9e-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="21a9e-135">Da un terminale eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="21a9e-135">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="21a9e-136">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="21a9e-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="21a9e-137">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="21a9e-137">Open the project</span></span>

<span data-ttu-id="21a9e-138">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="21a9e-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="21a9e-139">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="21a9e-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="21a9e-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21a9e-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="21a9e-141">Premere CTRL+F5 per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="21a9e-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="21a9e-142">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="21a9e-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="21a9e-143">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="21a9e-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="21a9e-144">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="21a9e-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="21a9e-145">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="21a9e-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="21a9e-146">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="21a9e-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="21a9e-147">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="21a9e-147">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="21a9e-148">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="21a9e-148">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="21a9e-150">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="21a9e-150">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="21a9e-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21a9e-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="21a9e-153">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="21a9e-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="21a9e-154">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="21a9e-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="21a9e-155">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="21a9e-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="21a9e-156">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="21a9e-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="21a9e-157">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="21a9e-157">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="21a9e-158">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="21a9e-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="21a9e-159">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="21a9e-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="21a9e-161">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="21a9e-161">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="21a9e-163">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="21a9e-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="21a9e-164">Premere **Cmd-Opt-F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="21a9e-164">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="21a9e-165">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="21a9e-165">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="21a9e-166">Nella home page dell'app selezionare **Accetto** per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="21a9e-166">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="21a9e-167">Questa app non tiene traccia delle informazioni personali, ma il modello di progetto include la funzionalità di consenso nel caso in cui risulti necessaria la conformità al [Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="21a9e-167">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="21a9e-169">La figura seguente visualizza l'app dopo che è stato impostato il consenso al rilevamento:</span><span class="sxs-lookup"><span data-stu-id="21a9e-169">The following image shows the app after you give consent to tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="21a9e-171">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="21a9e-171">Examine the project files</span></span>

<span data-ttu-id="21a9e-172">Di seguito viene visualizzata una panoramica delle cartelle e dei file principali del progetto, che verranno usati in esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="21a9e-172">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="21a9e-173">Cartella Pages</span><span class="sxs-lookup"><span data-stu-id="21a9e-173">Pages folder</span></span>

<span data-ttu-id="21a9e-174">Contiene le pagine Razor e i file di supporto.</span><span class="sxs-lookup"><span data-stu-id="21a9e-174">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="21a9e-175">Ogni pagina Razor è una coppia di file:</span><span class="sxs-lookup"><span data-stu-id="21a9e-175">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="21a9e-176">Un file con estensione *cshtml* contenente il markup HTML con codice C# che usa la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="21a9e-176">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="21a9e-177">Un file con estensione *cshtml.cs* contenente il codice C# per la gestione degli eventi della pagina.</span><span class="sxs-lookup"><span data-stu-id="21a9e-177">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="21a9e-178">I nomi dei file di supporto iniziano con un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="21a9e-178">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="21a9e-179">Ad esempio, il file *_Layout.cshtml* configura elementi dell'interfaccia utente comuni a tutte le pagine.</span><span class="sxs-lookup"><span data-stu-id="21a9e-179">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="21a9e-180">Questo file imposta il menu di navigazione nella parte superiore della pagina e le informazioni sul copyright in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="21a9e-180">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="21a9e-181">Per ulteriori informazioni, vedere <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="21a9e-181">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="21a9e-182">Cartella wwwroot</span><span class="sxs-lookup"><span data-stu-id="21a9e-182">wwwroot folder</span></span>

<span data-ttu-id="21a9e-183">Contiene i file statici, ad esempio i file HTML, i file JavaScript e i file CSS.</span><span class="sxs-lookup"><span data-stu-id="21a9e-183">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="21a9e-184">Per ulteriori informazioni, vedere <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="21a9e-184">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="21a9e-185">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="21a9e-185">appSettings.json</span></span>

<span data-ttu-id="21a9e-186">Contiene i dati di configurazione, ad esempio le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="21a9e-186">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="21a9e-187">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="21a9e-187">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="21a9e-188">Program.cs</span><span class="sxs-lookup"><span data-stu-id="21a9e-188">Program.cs</span></span>

<span data-ttu-id="21a9e-189">Contiene il punto di ingresso per il programma.</span><span class="sxs-lookup"><span data-stu-id="21a9e-189">Contains the entry point for the program.</span></span> <span data-ttu-id="21a9e-190">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="21a9e-190">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="21a9e-191">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="21a9e-191">Startup.cs</span></span>

<span data-ttu-id="21a9e-192">Contiene codice che configura il comportamento dell'app, ad esempio se richiede il consenso per i cookie.</span><span class="sxs-lookup"><span data-stu-id="21a9e-192">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="21a9e-193">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="21a9e-193">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21a9e-194">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="21a9e-194">Additional resources</span></span>

* [<span data-ttu-id="21a9e-195">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="21a9e-195">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="21a9e-196">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="21a9e-196">Next steps</span></span>

<span data-ttu-id="21a9e-197">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="21a9e-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21a9e-198">Creazione di un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="21a9e-198">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="21a9e-199">Esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="21a9e-199">Ran the app.</span></span>
> * <span data-ttu-id="21a9e-200">Esame dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="21a9e-200">Examined the project files.</span></span>

<span data-ttu-id="21a9e-201">Passare all'esercitazione successiva della serie:</span><span class="sxs-lookup"><span data-stu-id="21a9e-201">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="21a9e-202">Aggiungere un modello</span><span class="sxs-lookup"><span data-stu-id="21a9e-202">Add a model</span></span>](xref:tutorials/razor-pages/model)
