---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/24/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 96ef8fc055a6b92cd0808d02031d917b8446f305
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394750"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="effdf-103">Creare un'interfaccia utente riutilizzabile tramite il progetto di libreria di classi Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="effdf-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="effdf-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="effdf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="effdf-105">Le visualizzazioni Razor, pagine, i controller, modelli di pagina [componenti Razor](xref:blazor/class-libraries), [componenti di visualizzazione](xref:mvc/views/view-components), e i modelli di dati possono essere compilati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="effdf-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="effdf-106">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="effdf-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="effdf-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="effdf-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="effdf-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="effdf-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="effdf-109">Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="effdf-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="effdf-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="effdf-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="effdf-111">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="effdf-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="effdf-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="effdf-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="effdf-113">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="effdf-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="effdf-114">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="effdf-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="effdf-115">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="effdf-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="effdf-116">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="effdf-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="effdf-117">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="effdf-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="effdf-118">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="effdf-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="effdf-119">Un RCL ha file di progetto seguente:</span><span class="sxs-lookup"><span data-stu-id="effdf-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="effdf-120">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="effdf-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="effdf-121">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="effdf-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="effdf-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="effdf-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="effdf-123">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="effdf-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="effdf-124">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="effdf-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="effdf-125">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="effdf-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="effdf-126">I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella.</span><span class="sxs-lookup"><span data-stu-id="effdf-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="effdf-127">Visualizzare [layout delle pagine RCL](#afs) per creare contenuto in un RCL che espone `~/Pages` anziché `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="effdf-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="effdf-128">Riferimento ai contenuti RCL</span><span class="sxs-lookup"><span data-stu-id="effdf-128">Referencing RCL content</span></span>

<span data-ttu-id="effdf-129">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="effdf-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="effdf-130">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="effdf-130">NuGet package.</span></span> <span data-ttu-id="effdf-131">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="effdf-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="effdf-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="effdf-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="effdf-133">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="effdf-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="effdf-134">Procedura dettagliata: Creare un progetto RCL e usare da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="effdf-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="effdf-135">È possibile scaricare il [progetto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="effdf-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="effdf-136">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="effdf-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="effdf-137">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="effdf-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="effdf-138">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="effdf-138">Test the download app</span></span>

<span data-ttu-id="effdf-139">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="effdf-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="effdf-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="effdf-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="effdf-141">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="effdf-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="effdf-142">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="effdf-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="effdf-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="effdf-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="effdf-144">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="effdf-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="effdf-145">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="effdf-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="effdf-146">Seguire le istruzioni indicate in [Testare WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="effdf-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="effdf-147">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="effdf-147">Create an RCL</span></span>

<span data-ttu-id="effdf-148">In questa sezione viene creato un RCL.</span><span class="sxs-lookup"><span data-stu-id="effdf-148">In this section, an RCL is created.</span></span> <span data-ttu-id="effdf-149">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="effdf-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="effdf-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="effdf-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="effdf-151">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="effdf-151">Create the RCL project:</span></span>

* <span data-ttu-id="effdf-152">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="effdf-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="effdf-153">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="effdf-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="effdf-154">Denominare l'app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="effdf-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="effdf-155">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="effdf-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="effdf-156">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="effdf-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="effdf-157">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="effdf-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="effdf-158">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="effdf-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="effdf-159">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="effdf-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="effdf-160">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="effdf-160">The preceding commands:</span></span>

* <span data-ttu-id="effdf-161">Crea il `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="effdf-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="effdf-162">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="effdf-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="effdf-163">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="effdf-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="effdf-164">Crea una [viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) file e lo aggiunge al RCL.</span><span class="sxs-lookup"><span data-stu-id="effdf-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="effdf-165">Il *viewstart. cshtml* file è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).</span><span class="sxs-lookup"><span data-stu-id="effdf-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="effdf-166">Aggiungere i file Razor e cartelle per il progetto</span><span class="sxs-lookup"><span data-stu-id="effdf-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="effdf-167">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="effdf-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="effdf-168">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="effdf-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="effdf-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="effdf-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="effdf-170">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="effdf-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="effdf-171">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="effdf-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="effdf-172">Per ulteriori informazioni sul *viewimports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="effdf-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="effdf-173">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="effdf-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="effdf-174">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="effdf-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="effdf-175">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="effdf-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="effdf-176">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="effdf-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="effdf-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="effdf-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="effdf-178">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="effdf-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="effdf-179">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >  **Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="effdf-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="effdf-180">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="effdf-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="effdf-181">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="effdf-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="effdf-182">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="effdf-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="effdf-183">Selezionare **Applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="effdf-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="effdf-184">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="effdf-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="effdf-185">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Dipendenze di compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="effdf-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="effdf-186">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="effdf-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="effdf-187">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e scegliere **Aggiungi** > **Riferimento**.</span><span class="sxs-lookup"><span data-stu-id="effdf-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="effdf-188">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="effdf-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="effdf-189">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="effdf-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="effdf-190">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="effdf-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="effdf-191">Creare un'app web Razor Pages e un file di soluzione che contiene l'app Razor Pages e la RCL:</span><span class="sxs-lookup"><span data-stu-id="effdf-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="effdf-192">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="effdf-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="effdf-193">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="effdf-193">Test WebApp1</span></span>

<span data-ttu-id="effdf-194">Verificare che la libreria di classi Razor dell'interfaccia utente sia in uso:</span><span class="sxs-lookup"><span data-stu-id="effdf-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="effdf-195">Passare a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="effdf-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="effdf-196">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="effdf-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="effdf-197">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="effdf-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="effdf-198">Ad esempio, aggiungere *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* per App Web 1, e la pagina Page1 nell'App Web 1 avrà la precedenza sulla pagina Page1 nella RCL.</span><span class="sxs-lookup"><span data-stu-id="effdf-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="effdf-199">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="effdf-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="effdf-200">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="effdf-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="effdf-201">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="effdf-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="effdf-202">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="effdf-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="effdf-203">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="effdf-203">RCL Pages layout</span></span>

<span data-ttu-id="effdf-204">Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:</span><span class="sxs-lookup"><span data-stu-id="effdf-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="effdf-205">*RazorUIClassLib o delle pagine*</span><span class="sxs-lookup"><span data-stu-id="effdf-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="effdf-206">*RazorUIClassLib/pagine/Shared*</span><span class="sxs-lookup"><span data-stu-id="effdf-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="effdf-207">Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="effdf-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="effdf-208">Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="effdf-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="effdf-209">Creare un RCL con Asset statici</span><span class="sxs-lookup"><span data-stu-id="effdf-209">Create an RCL with static assets</span></span>

<span data-ttu-id="effdf-210">Un RCL potrebbero richiedere asset statici complementari che possono essere utilizzati dalle app dispendiosa in termini del RCL.</span><span class="sxs-lookup"><span data-stu-id="effdf-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="effdf-211">ASP.NET Core consente di creare RCLs che includono asset statici disponibili per un'app consumer.</span><span class="sxs-lookup"><span data-stu-id="effdf-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="effdf-212">Per includere gli asset complementare come parte di un RCL, creare un *wwwroot* cartella nella libreria di classi e includere eventuali file richiesti in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="effdf-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="effdf-213">La creazione di un RCL, companion in tutti gli asset nel *wwwroot* cartella vengono automaticamente inclusi nel pacchetto e vengono rese disponibile per le app che fa riferimento il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="effdf-213">When packing an RCL, all companion assets in the *wwwroot* folder are included in the package automatically and are made available to apps referencing the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="effdf-214">Utilizzare il contenuto di un riferimento RCL</span><span class="sxs-lookup"><span data-stu-id="effdf-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="effdf-215">I file inclusi nel *wwwroot* cartella della RCL vengono esposte all'app dispendiosa in termini di sotto del prefisso `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="effdf-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="effdf-216">`{LIBRARY NAME}` è il nome del progetto libreria convertito in lettere minuscole con periodi (`.`) rimosso.</span><span class="sxs-lookup"><span data-stu-id="effdf-216">`{LIBRARY NAME}` is the library project name converted to lowercase with periods (`.`) removed.</span></span> <span data-ttu-id="effdf-217">Ad esempio, una libreria denominata *Razor.Class.Lib* risultante in un percorso per il contenuto statico in `_content/razorclasslib/`.</span><span class="sxs-lookup"><span data-stu-id="effdf-217">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/razorclasslib/`.</span></span>

<span data-ttu-id="effdf-218">Le app consumer fa riferimento all'asset statici forniti dalla libreria con `<script>`, `<style>`, `<img>`e gli altri tag HTML.</span><span class="sxs-lookup"><span data-stu-id="effdf-218">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="effdf-219">Le app consumer deve avere [supporto dei file statici](xref:fundamentals/static-files) abilitata.</span><span class="sxs-lookup"><span data-stu-id="effdf-219">The consuming app must have [static file support](xref:fundamentals/static-files) enabled.</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="effdf-220">Flusso di sviluppo basati su più progetti</span><span class="sxs-lookup"><span data-stu-id="effdf-220">Multi-project development flow</span></span>

<span data-ttu-id="effdf-221">Quando viene eseguita l'app dispendiosa in termini di:</span><span class="sxs-lookup"><span data-stu-id="effdf-221">When the consuming app runs:</span></span>

* <span data-ttu-id="effdf-222">Le risorse nel RCL si trovino nelle rispettive cartelle originali.</span><span class="sxs-lookup"><span data-stu-id="effdf-222">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="effdf-223">Gli asset non vengono spostati all'app consumer.</span><span class="sxs-lookup"><span data-stu-id="effdf-223">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="effdf-224">Qualsiasi modifica apportata all'interno di RCL *wwwroot* cartella viene riflessa nell'app consumer dopo il RCL viene ricompilato e senza ricompilare l'app consumer.</span><span class="sxs-lookup"><span data-stu-id="effdf-224">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="effdf-225">Quando viene compilato il RCL, viene generato un manifesto che descrive le posizioni di asset web statico.</span><span class="sxs-lookup"><span data-stu-id="effdf-225">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="effdf-226">Le app consumer legge il manifesto in fase di esecuzione per utilizzare le risorse da cui viene fatto riferimento progetti e pacchetti.</span><span class="sxs-lookup"><span data-stu-id="effdf-226">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="effdf-227">Quando un nuovo asset viene aggiunto a un RCL, è necessario ricompilare il RCL per aggiornare il relativo manifesto prima che un'app consumer possa accedere il nuovo asset.</span><span class="sxs-lookup"><span data-stu-id="effdf-227">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="effdf-228">Pubblica</span><span class="sxs-lookup"><span data-stu-id="effdf-228">Publish</span></span>

<span data-ttu-id="effdf-229">Quando l'app viene pubblicata, gli asset complementare da tutti i progetti e riferimento i pacchetti vengono copiati le *wwwroot* cartella dell'app pubblicata in `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="effdf-229">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>
