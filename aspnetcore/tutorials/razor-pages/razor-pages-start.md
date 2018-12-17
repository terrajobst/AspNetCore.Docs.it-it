---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861628"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="08b87-104">Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08b87-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="08b87-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="08b87-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="08b87-106">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08b87-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="08b87-107">L'app gestisce un database di titoli di film.</span><span class="sxs-lookup"><span data-stu-id="08b87-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="08b87-108">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="08b87-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08b87-109">Creare un'app Web di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08b87-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="08b87-110">Aggiungere un modello ed eseguirne lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="08b87-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="08b87-111">Usare un database.</span><span class="sxs-lookup"><span data-stu-id="08b87-111">Work with a database.</span></span>
> * <span data-ttu-id="08b87-112">Aggiungere ricerca e convalida.</span><span class="sxs-lookup"><span data-stu-id="08b87-112">Add search and validation.</span></span>

<span data-ttu-id="08b87-113">Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare gli elementi costituiti da titoli di film.</span><span class="sxs-lookup"><span data-stu-id="08b87-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="08b87-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="08b87-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="08b87-115">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="08b87-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08b87-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08b87-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="08b87-117">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="08b87-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="08b87-118">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08b87-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="08b87-119">Denominare il progetto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="08b87-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="08b87-120">È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="08b87-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="08b87-121">![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="08b87-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="08b87-122">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="08b87-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="08b87-124">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="08b87-124">The following starter project is created:</span></span>

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="08b87-126">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="08b87-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="08b87-127">Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="08b87-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="08b87-128">La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="08b87-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="08b87-129">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="08b87-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="08b87-130">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="08b87-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="08b87-131">Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale.</span><span class="sxs-lookup"><span data-stu-id="08b87-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="08b87-132">Nell'immagine precedente, il numero di porta è 5001.</span><span class="sxs-lookup"><span data-stu-id="08b87-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="08b87-133">Quando si esegue l'app verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="08b87-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="08b87-134">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="08b87-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="08b87-135">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="08b87-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08b87-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08b87-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="08b87-137">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="08b87-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="08b87-138">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="08b87-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="08b87-139">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="08b87-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="08b87-140">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="08b87-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="08b87-141">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="08b87-141">Select **Yes**</span></span>

  * <span data-ttu-id="08b87-142">`dotnet new webapp -o RazorPagesMovie`: crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="08b87-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="08b87-143">`code -r RazorPagesMovie`: carica il file di progetto *RazorPagesMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="08b87-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="08b87-144">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="08b87-144">Launch the app</span></span>

* <span data-ttu-id="08b87-145">Premere **CTRL+F5** per l'esecuzione senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="08b87-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="08b87-146">Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="08b87-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="08b87-147">La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="08b87-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="08b87-148">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="08b87-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="08b87-149">Localhost viene usato solo per le richieste web del computer locale.</span><span class="sxs-lookup"><span data-stu-id="08b87-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="08b87-150">Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice.</span><span class="sxs-lookup"><span data-stu-id="08b87-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="08b87-151">Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="08b87-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08b87-152">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08b87-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="08b87-153">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="08b87-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="08b87-154">I comandi precedenti usano [.NET Core CLI](/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="08b87-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="08b87-155">Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="08b87-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="08b87-156">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="08b87-156">Open the project</span></span>

<span data-ttu-id="08b87-157">Premere Ctrl + C per arrestare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="08b87-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="08b87-158">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="08b87-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="08b87-159">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="08b87-159">Launch the app</span></span>

<span data-ttu-id="08b87-160">Per avviare l'app, selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="08b87-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="08b87-161">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="08b87-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="08b87-162">Selezionare **Accept** (Accetto) per autorizzare il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="08b87-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="08b87-163">Questa app non rileva informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="08b87-163">This app doesn't track personal information.</span></span> <span data-ttu-id="08b87-164">Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="08b87-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="08b87-166">La figura seguente illustra l'app dopo aver accettato il rilevamento:</span><span class="sxs-lookup"><span data-stu-id="08b87-166">The following image shows the app after accepting tracking:</span></span>

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="08b87-168">File e cartelle di progetto</span><span class="sxs-lookup"><span data-stu-id="08b87-168">Project files and folders</span></span>

<span data-ttu-id="08b87-169">La tabella seguente elenca i file e le cartelle nel progetto.</span><span class="sxs-lookup"><span data-stu-id="08b87-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="08b87-170">A questo punto dell'esercitazione, il file più importante da comprendere è *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="08b87-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="08b87-171">Non è necessario rivedere ogni collegamento indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="08b87-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="08b87-172">I collegamenti sono forniti come riferimento quando sono necessarie altre informazioni su un file o una cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="08b87-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="08b87-173">File o cartella</span><span class="sxs-lookup"><span data-stu-id="08b87-173">File or folder</span></span>              | <span data-ttu-id="08b87-174">Scopo</span><span class="sxs-lookup"><span data-stu-id="08b87-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="08b87-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="08b87-175">*wwwroot*</span></span> | <span data-ttu-id="08b87-176">Contiene file statici.</span><span class="sxs-lookup"><span data-stu-id="08b87-176">Contains static files.</span></span> <span data-ttu-id="08b87-177">Vedere [File statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="08b87-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="08b87-178">*Pagine*</span><span class="sxs-lookup"><span data-stu-id="08b87-178">*Pages*</span></span> | <span data-ttu-id="08b87-179">Cartella per [Pagine Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="08b87-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="08b87-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="08b87-180">*appsettings.json*</span></span> | [<span data-ttu-id="08b87-181">Configurazione</span><span class="sxs-lookup"><span data-stu-id="08b87-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="08b87-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="08b87-182">*Program.cs*</span></span> | <span data-ttu-id="08b87-183">[Ospita](xref:fundamentals/host/index) l'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08b87-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="08b87-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="08b87-184">*Startup.cs*</span></span> | <span data-ttu-id="08b87-185">Configura i servizi e la pipeline della richiesta.</span><span class="sxs-lookup"><span data-stu-id="08b87-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="08b87-186">Vedere [Avvio](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="08b87-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="08b87-187">Cartella delle pagine</span><span class="sxs-lookup"><span data-stu-id="08b87-187">The Pages folder</span></span>

<span data-ttu-id="08b87-188">Il file *_Layout.cshtml* contiene elementi HTML comuni (script e fogli di stile) e imposta il layout per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="08b87-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="08b87-189">Ad esempio, facendo clic su **RazorPagesMovie**, **Home**, o **Privacy**, vengono visualizzati gli stessi elementi.</span><span class="sxs-lookup"><span data-stu-id="08b87-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="08b87-190">Gli elementi comuni includono il menu di navigazione nella parte superiore e l'intestazione nella parte inferiore della finestra.</span><span class="sxs-lookup"><span data-stu-id="08b87-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="08b87-191">Vedere [Layout](xref:mvc/views/layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="08b87-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="08b87-192">Il file *_ViewImports.cshtml* contiene le direttive Razor che vengono importate in ogni pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="08b87-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="08b87-193">Per altre informazioni, vedere [Importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="08b87-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="08b87-194">*_ViewStart.cshtml* imposta la proprietà pagine Razor `Layout` per l'uso del file *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08b87-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="08b87-195">Vedere [Layout](xref:mvc/views/layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="08b87-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="08b87-196">Il file *_ValidationScriptsPartial.cshtml* fornisce un riferimento agli script di convalida [jQuery](https://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="08b87-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="08b87-197">Quando si aggiungono le pagine `Create` e `Edit` in un secondo momento nell'esercitazione, viene usato il file *_ValidationScriptsPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08b87-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="08b87-198">Le pagine `Index`, `Error`, e `Privacy` vengono specificate per le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="08b87-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="08b87-199">`Index`: avvia un'app.</span><span class="sxs-lookup"><span data-stu-id="08b87-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="08b87-200">`Error`: visualizza informazioni sull'errore.</span><span class="sxs-lookup"><span data-stu-id="08b87-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="08b87-201">`Privacy`: specifica i dettagli dell'informativa sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="08b87-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="08b87-202">Per questa esercitazione, queste pagine non vengono usate.</span><span class="sxs-lookup"><span data-stu-id="08b87-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="08b87-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08b87-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="08b87-204">Premere F7 per spostarsi tra una pagina Razor Pages e il PageModel</span><span class="sxs-lookup"><span data-stu-id="08b87-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="08b87-205">Per passare da una pagina Razor (file con estensione *\*cshtml*) al file C# (con estensione *\*cshtml.cs*), premere F7.</span><span class="sxs-lookup"><span data-stu-id="08b87-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="08b87-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="08b87-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="08b87-207">Per convenzione, la pagina Razor (file con estensione *\*cshtml*) e l'elemento `PageModel` associato hanno lo stesso nome file radice.</span><span class="sxs-lookup"><span data-stu-id="08b87-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="08b87-208">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="08b87-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="08b87-209">Per convenzione, la pagina Razor (file con estensione *\*cshtml*) e l'elemento `PageModel` associato hanno lo stesso nome file radice.</span><span class="sxs-lookup"><span data-stu-id="08b87-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="08b87-210">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="08b87-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)