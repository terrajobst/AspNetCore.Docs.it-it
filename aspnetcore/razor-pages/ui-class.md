---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: In questo articolo viene illustrato come creare un'interfaccia utente Razor riutilizzabile in una libreria di classi.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 3adad70e31089b959192ed0c5828f294c696b070
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523116"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="eae30-103">Creare un'interfaccia utente riutilizzabile usando il progetto libreria di classi Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eae30-103">Create reusable UI using the Razor Class Library project in ASP.NET Core</span></span>

<span data-ttu-id="eae30-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eae30-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eae30-105">È possibile compilare in una libreria di classi Razor visualizzazioni, pagine, controller, modelli di pagine, [Componenti di visualizzazione](xref:mvc/views/view-components) e modelli di dati Razor.</span><span class="sxs-lookup"><span data-stu-id="eae30-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="eae30-106">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="eae30-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="eae30-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="eae30-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="eae30-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="eae30-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="eae30-109">Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="eae30-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="eae30-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eae30-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="eae30-111">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="eae30-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eae30-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eae30-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="eae30-113">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="eae30-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="eae30-114">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="eae30-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="eae30-115">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="eae30-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="eae30-116">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="eae30-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="eae30-117">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="eae30-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="eae30-118">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="eae30-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="eae30-119">Una libreria di classi Razor è il seguente file di progetto:</span><span class="sxs-lookup"><span data-stu-id="eae30-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="eae30-120">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="eae30-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="eae30-121">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="eae30-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="eae30-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="eae30-122">For example:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="eae30-123">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="eae30-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="eae30-124">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="eae30-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="eae30-125">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="eae30-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="eae30-126">I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella.</span><span class="sxs-lookup"><span data-stu-id="eae30-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="eae30-127">Visualizzare [layout delle pagine RCL](#afs) per creare contenuto in un RCL che espone `~/Pages` anziché `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="eae30-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="eae30-128">Riferimento ai contenuti della libreria di classi Razor</span><span class="sxs-lookup"><span data-stu-id="eae30-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="eae30-129">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="eae30-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="eae30-130">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="eae30-130">NuGet package.</span></span> <span data-ttu-id="eae30-131">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="eae30-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="eae30-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="eae30-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="eae30-133">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="eae30-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="eae30-134">Procedura dettagliata: Creare un progetto libreria di classi Razor e usarlo da un progetto di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="eae30-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="eae30-135">È possibile scaricare il [progetto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="eae30-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="eae30-136">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="eae30-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="eae30-137">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="eae30-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="eae30-138">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="eae30-138">Test the download app</span></span>

<span data-ttu-id="eae30-139">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="eae30-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eae30-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eae30-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eae30-141">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eae30-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="eae30-142">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="eae30-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="eae30-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="eae30-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="eae30-144">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="eae30-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```console
dotnet build
```

<span data-ttu-id="eae30-145">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="eae30-145">Move to the *WebApp1* directory and run the app:</span></span>

```console
dotnet run
```

------

<span data-ttu-id="eae30-146">Seguire le istruzioni indicate in [Testare WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="eae30-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="eae30-147">Creare una libreria di classi Razor</span><span class="sxs-lookup"><span data-stu-id="eae30-147">Create a Razor Class Library</span></span>

<span data-ttu-id="eae30-148">In questa sezione viene creata una libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="eae30-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="eae30-149">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="eae30-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eae30-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eae30-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eae30-151">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="eae30-151">Create the RCL project:</span></span>

* <span data-ttu-id="eae30-152">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="eae30-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="eae30-153">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="eae30-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="eae30-154">Denominare l'app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="eae30-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="eae30-155">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="eae30-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="eae30-156">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="eae30-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="eae30-157">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eae30-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="eae30-158">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="eae30-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="eae30-159">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="eae30-159">From the command line, run the following:</span></span>

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="eae30-160">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="eae30-160">The preceding commands:</span></span>

* <span data-ttu-id="eae30-161">Crea la libreria di classi Razor `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="eae30-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="eae30-162">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="eae30-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="eae30-163">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="eae30-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="eae30-164">Crea una [viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) file e lo aggiunge al RCL.</span><span class="sxs-lookup"><span data-stu-id="eae30-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="eae30-165">Il *viewstart. cshtml* file è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).</span><span class="sxs-lookup"><span data-stu-id="eae30-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="eae30-166">Aggiungere i file Razor e cartelle per il progetto</span><span class="sxs-lookup"><span data-stu-id="eae30-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="eae30-167">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="eae30-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="eae30-168">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="eae30-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="eae30-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="eae30-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="eae30-170">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eae30-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="eae30-171">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="eae30-171">For example:</span></span>

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="eae30-172">Per ulteriori informazioni sul *viewimports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="eae30-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="eae30-173">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="eae30-173">Build the class library to verify there are no compiler errors:</span></span>

```console
dotnet build RazorUIClassLib
```

<span data-ttu-id="eae30-174">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="eae30-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="eae30-175">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="eae30-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="eae30-176">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="eae30-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eae30-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eae30-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eae30-178">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="eae30-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="eae30-179">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >  **Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="eae30-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="eae30-180">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="eae30-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="eae30-181">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="eae30-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="eae30-182">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="eae30-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="eae30-183">Selezionare **Applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="eae30-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="eae30-184">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="eae30-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="eae30-185">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Dipendenze di compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="eae30-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="eae30-186">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="eae30-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="eae30-187">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e scegliere **Aggiungi** > **Riferimento**.</span><span class="sxs-lookup"><span data-stu-id="eae30-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="eae30-188">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="eae30-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="eae30-189">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="eae30-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="eae30-190">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="eae30-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="eae30-191">Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e la libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="eae30-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="eae30-192">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="eae30-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="eae30-193">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="eae30-193">Test WebApp1</span></span>

<span data-ttu-id="eae30-194">Verificare che la libreria di classi dell'interfaccia utente Razor sia in uso.</span><span class="sxs-lookup"><span data-stu-id="eae30-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="eae30-195">Passare a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="eae30-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="eae30-196">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="eae30-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="eae30-197">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="eae30-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="eae30-198">Ad esempio, aggiungere *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* per App Web 1, e la pagina Page1 nell'App Web 1 avrà la precedenza sulla pagina Page1 nella libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="eae30-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the Razor Class Library.</span></span>

<span data-ttu-id="eae30-199">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="eae30-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="eae30-200">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eae30-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="eae30-201">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="eae30-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="eae30-202">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="eae30-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="eae30-203">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="eae30-203">RCL Pages layout</span></span>

<span data-ttu-id="eae30-204">Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:</span><span class="sxs-lookup"><span data-stu-id="eae30-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="eae30-205">*RazorUIClassLib o delle pagine*</span><span class="sxs-lookup"><span data-stu-id="eae30-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="eae30-206">*RazorUIClassLib/pagine/Shared*</span><span class="sxs-lookup"><span data-stu-id="eae30-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="eae30-207">Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eae30-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="eae30-208">Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="eae30-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>
  
```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```
