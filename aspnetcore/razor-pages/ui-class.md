---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 468d961c291810ca4dfbe615acd972cfd6e7572a
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886399"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="8ae3d-103">Creare un'interfaccia utente riutilizzabile usando il progetto libreria di classi Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ae3d-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="8ae3d-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ae3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8ae3d-105">Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="8ae3d-106">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="8ae3d-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="8ae3d-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="8ae3d-109">Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="8ae3d-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="8ae3d-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8ae3d-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="8ae3d-111">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="8ae3d-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ae3d-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ae3d-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8ae3d-113">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8ae3d-114">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8ae3d-115">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="8ae3d-116">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="8ae3d-117">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="8ae3d-118">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="8ae3d-119">Un RCL ha il seguente file di progetto:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8ae3d-120">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ae3d-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8ae3d-121">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="8ae3d-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="8ae3d-123">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="8ae3d-124">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="8ae3d-125">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="8ae3d-126">I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="8ae3d-127">Vedere [layout di pagine RCL](#afs) per creare un RCL che espone il `~/Pages` contenuto in `~/Areas/Pages`anziché.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="8ae3d-128">Riferimento al contenuto di RCL</span><span class="sxs-lookup"><span data-stu-id="8ae3d-128">Referencing RCL content</span></span>

<span data-ttu-id="8ae3d-129">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="8ae3d-130">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-130">NuGet package.</span></span> <span data-ttu-id="8ae3d-131">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="8ae3d-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="8ae3d-133">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="8ae3d-134">Procedura dettagliata: Creare un progetto RCL e usarlo da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8ae3d-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="8ae3d-135">È possibile scaricare il [progetto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="8ae3d-136">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="8ae3d-137">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="8ae3d-138">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="8ae3d-138">Test the download app</span></span>

<span data-ttu-id="8ae3d-139">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ae3d-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ae3d-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ae3d-141">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="8ae3d-142">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8ae3d-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ae3d-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8ae3d-144">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="8ae3d-145">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

---

<span data-ttu-id="8ae3d-146">Seguire le istruzioni indicate in [Testare WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="8ae3d-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="8ae3d-147">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="8ae3d-147">Create an RCL</span></span>

<span data-ttu-id="8ae3d-148">In questa sezione viene creato un RCL.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-148">In this section, an RCL is created.</span></span> <span data-ttu-id="8ae3d-149">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ae3d-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ae3d-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ae3d-151">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-151">Create the RCL project:</span></span>

* <span data-ttu-id="8ae3d-152">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8ae3d-153">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8ae3d-154">Denominare l'app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="8ae3d-155">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="8ae3d-156">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="8ae3d-157">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8ae3d-158">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ae3d-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8ae3d-159">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="8ae3d-160">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-160">The preceding commands:</span></span>

* <span data-ttu-id="8ae3d-161">Crea l' `RazorUIClassLib` oggetto RCL.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="8ae3d-162">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="8ae3d-163">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="8ae3d-164">Crea una [viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) file e lo aggiunge al RCL.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="8ae3d-165">Il *viewstart. cshtml* file è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="8ae3d-166">Aggiungere i file Razor e cartelle per il progetto</span><span class="sxs-lookup"><span data-stu-id="8ae3d-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="8ae3d-167">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="8ae3d-168">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="8ae3d-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="8ae3d-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="8ae3d-170">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="8ae3d-171">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="8ae3d-172">Per ulteriori informazioni sul *viewimports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="8ae3d-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="8ae3d-173">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="8ae3d-174">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="8ae3d-175">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="8ae3d-176">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="8ae3d-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ae3d-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ae3d-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8ae3d-178">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="8ae3d-179">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >  **Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="8ae3d-180">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8ae3d-181">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="8ae3d-182">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="8ae3d-183">Selezionare **Applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="8ae3d-184">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="8ae3d-185">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Dipendenze di compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="8ae3d-186">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="8ae3d-187">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e scegliere **Aggiungi** > **Riferimento**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="8ae3d-188">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="8ae3d-189">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8ae3d-190">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ae3d-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8ae3d-191">Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e il RCL:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="8ae3d-192">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="8ae3d-193">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="8ae3d-193">Test WebApp1</span></span>

<span data-ttu-id="8ae3d-194">Verificare che la libreria di classi dell'interfaccia utente Razor sia in uso:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="8ae3d-195">Passare a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="8ae3d-196">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="8ae3d-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="8ae3d-197">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="8ae3d-198">Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="8ae3d-199">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="8ae3d-200">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="8ae3d-201">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="8ae3d-202">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="8ae3d-203">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="8ae3d-203">RCL Pages layout</span></span>

<span data-ttu-id="8ae3d-204">Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="8ae3d-205">*RazorUIClassLib o delle pagine*</span><span class="sxs-lookup"><span data-stu-id="8ae3d-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="8ae3d-206">*RazorUIClassLib/pagine/Shared*</span><span class="sxs-lookup"><span data-stu-id="8ae3d-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="8ae3d-207">Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="8ae3d-208">Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="8ae3d-209">Creare un RCL con asset statici</span><span class="sxs-lookup"><span data-stu-id="8ae3d-209">Create an RCL with static assets</span></span>

<span data-ttu-id="8ae3d-210">Un RCL può richiedere risorse statiche complementari a cui è possibile fare riferimento dall'app consumer di RCL.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="8ae3d-211">ASP.NET Core consente di creare RCL che includono risorse statiche disponibili per un'app di consumo.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="8ae3d-212">Per includere asset complementari come parte di un RCL, creare una cartella *wwwroot* nella libreria di classi e includere tutti i file necessari in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="8ae3d-213">Quando si esegue la compressione di un RCL, tutti gli asset complementari nella cartella *wwwroot* vengono inclusi automaticamente nel pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-213">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="8ae3d-214">Utilizzare il contenuto da un RCL a cui si fa riferimento</span><span class="sxs-lookup"><span data-stu-id="8ae3d-214">Consume content from a referenced RCL</span></span>

<span data-ttu-id="8ae3d-215">I file inclusi nella cartella *wwwroot* di RCL vengono esposti all'app consumer sotto il prefisso `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-215">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="8ae3d-216">Ad esempio, una libreria denominata *Razor. Class. lib* genera un percorso al contenuto statico in `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-216">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="8ae3d-217">L'app consumer fa riferimento a risorse statiche fornite dalla libreria `<script>`con `<style>` `<img>`,, e altri tag HTML.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-217">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="8ae3d-218">Per l'app consumer deve essere abilitato il [supporto file statico](xref:fundamentals/static-files) in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-218">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="8ae3d-219">Quando si esegue l'app consumer dall'output di`dotnet run`compilazione (), le risorse Web statiche sono abilitate per impostazione predefinita nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-219">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="8ae3d-220">Per supportare asset in altri ambienti durante l'esecuzione dall'output di compilazione `UseStaticWebAssets` , chiamare sul generatore host in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-220">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="8ae3d-221">Quando `UseStaticWebAssets` si esegue un'app dall'output pubblicato (`dotnet publish`), la chiamata non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-221">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="8ae3d-222">Flusso di sviluppo per più progetti</span><span class="sxs-lookup"><span data-stu-id="8ae3d-222">Multi-project development flow</span></span>

<span data-ttu-id="8ae3d-223">Quando viene eseguita l'app consumer:</span><span class="sxs-lookup"><span data-stu-id="8ae3d-223">When the consuming app runs:</span></span>

* <span data-ttu-id="8ae3d-224">Gli asset nel RCL rimanere nelle cartelle originali.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-224">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="8ae3d-225">Gli asset non vengono spostati nell'app consumer.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-225">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="8ae3d-226">Tutte le modifiche apportate all'interno della cartella *wwwroot* di RCL vengono riflesse nell'app consumer dopo che il RCL viene ricompilato e senza ricompilare l'app consumer.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-226">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="8ae3d-227">Quando viene compilato il RCL, viene generato un manifesto che descrive i percorsi di asset Web statici.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-227">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="8ae3d-228">L'app consumer legge il manifesto in fase di esecuzione per usare gli asset dei progetti e dei pacchetti a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-228">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="8ae3d-229">Quando si aggiunge un nuovo asset a un RCL, è necessario ricompilare il RCL per aggiornare il manifesto prima che un'app che utilizza possa accedere al nuovo asset.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-229">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="8ae3d-230">Pubblica</span><span class="sxs-lookup"><span data-stu-id="8ae3d-230">Publish</span></span>

<span data-ttu-id="8ae3d-231">Quando viene pubblicata l'app, gli asset complementari di tutti i progetti e i pacchetti a cui viene fatto riferimento vengono copiati nella cartella wwwroot `_content/{LIBRARY NAME}/`dell'app pubblicata in.</span><span class="sxs-lookup"><span data-stu-id="8ae3d-231">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
