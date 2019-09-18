---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/22/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 92c04c1ac4c70c6245accf272753bc914aaab860
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081868"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="d49de-103">Creare un'interfaccia utente riutilizzabile usando il progetto libreria di classi Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d49de-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="d49de-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d49de-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d49de-105">Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="d49de-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="d49de-106">La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente.</span><span class="sxs-lookup"><span data-stu-id="d49de-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="d49de-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="d49de-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="d49de-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="d49de-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="d49de-109">Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="d49de-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="d49de-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d49de-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="d49de-111">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="d49de-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d49de-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d49de-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d49de-113">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="d49de-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d49de-114">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d49de-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d49de-115">Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d49de-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="d49de-116">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="d49de-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="d49de-117">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="d49de-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="d49de-118">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d49de-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="d49de-119">Un RCL ha il seguente file di progetto:</span><span class="sxs-lookup"><span data-stu-id="d49de-119">An RCL has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d49de-120">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d49de-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d49de-121">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d49de-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="d49de-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d49de-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="d49de-123">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d49de-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="d49de-124">Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.</span><span class="sxs-lookup"><span data-stu-id="d49de-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="d49de-125">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="d49de-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="d49de-126">I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella.</span><span class="sxs-lookup"><span data-stu-id="d49de-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="d49de-127">Vedere [layout di pagine RCL](#afs) per creare un RCL che espone il `~/Pages` contenuto in `~/Areas/Pages`anziché.</span><span class="sxs-lookup"><span data-stu-id="d49de-127">See [RCL Pages layout](#afs) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-rcl-content"></a><span data-ttu-id="d49de-128">Riferimento al contenuto di RCL</span><span class="sxs-lookup"><span data-stu-id="d49de-128">Referencing RCL content</span></span>

<span data-ttu-id="d49de-129">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="d49de-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="d49de-130">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="d49de-130">NuGet package.</span></span> <span data-ttu-id="d49de-131">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="d49de-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="d49de-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d49de-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="d49de-133">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="d49de-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="d49de-134">Procedura dettagliata: Creare un progetto RCL e usarlo da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d49de-134">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="d49de-135">È possibile scaricare il [progetto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="d49de-135">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="d49de-136">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="d49de-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="d49de-137">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="d49de-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="d49de-138">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="d49de-138">Test the download app</span></span>

<span data-ttu-id="d49de-139">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="d49de-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d49de-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d49de-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d49de-141">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d49de-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="d49de-142">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d49de-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d49de-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d49de-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d49de-144">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="d49de-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="d49de-145">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="d49de-145">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="d49de-146">Seguire le istruzioni indicate in [Testare WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="d49de-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="d49de-147">Creare un RCL</span><span class="sxs-lookup"><span data-stu-id="d49de-147">Create an RCL</span></span>

<span data-ttu-id="d49de-148">In questa sezione viene creato un RCL.</span><span class="sxs-lookup"><span data-stu-id="d49de-148">In this section, an RCL is created.</span></span> <span data-ttu-id="d49de-149">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="d49de-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d49de-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d49de-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d49de-151">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="d49de-151">Create the RCL project:</span></span>

* <span data-ttu-id="d49de-152">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="d49de-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d49de-153">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d49de-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d49de-154">Denominare l'app **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d49de-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="d49de-155">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="d49de-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="d49de-156">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d49de-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="d49de-157">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d49de-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d49de-158">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d49de-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d49de-159">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="d49de-159">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="d49de-160">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="d49de-160">The preceding commands:</span></span>

* <span data-ttu-id="d49de-161">Crea l' `RazorUIClassLib` oggetto RCL.</span><span class="sxs-lookup"><span data-stu-id="d49de-161">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="d49de-162">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="d49de-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="d49de-163">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="d49de-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="d49de-164">Crea una [viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) file e lo aggiunge al RCL.</span><span class="sxs-lookup"><span data-stu-id="d49de-164">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="d49de-165">Il *viewstart. cshtml* file è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).</span><span class="sxs-lookup"><span data-stu-id="d49de-165">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="d49de-166">Aggiungere i file Razor e cartelle per il progetto</span><span class="sxs-lookup"><span data-stu-id="d49de-166">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="d49de-167">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d49de-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="d49de-168">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d49de-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="d49de-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="d49de-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="d49de-170">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d49de-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d49de-171">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d49de-171">For example:</span></span>

```dotnetcli
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="d49de-172">Per ulteriori informazioni sul *viewimports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="d49de-172">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="d49de-173">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="d49de-173">Build the class library to verify there are no compiler errors:</span></span>

```dotnetcli
dotnet build RazorUIClassLib
```

<span data-ttu-id="d49de-174">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="d49de-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="d49de-175">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="d49de-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="d49de-176">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d49de-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d49de-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d49de-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d49de-178">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="d49de-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="d49de-179">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >  **Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="d49de-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="d49de-180">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d49de-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d49de-181">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="d49de-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="d49de-182">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="d49de-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="d49de-183">Selezionare **Applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d49de-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="d49de-184">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="d49de-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="d49de-185">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Dipendenze di compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="d49de-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="d49de-186">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="d49de-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="d49de-187">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e scegliere **Aggiungi** > **Riferimento**.</span><span class="sxs-lookup"><span data-stu-id="d49de-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="d49de-188">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="d49de-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="d49de-189">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d49de-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d49de-190">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d49de-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d49de-191">Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e il RCL:</span><span class="sxs-lookup"><span data-stu-id="d49de-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="d49de-192">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="d49de-192">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="d49de-193">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="d49de-193">Test WebApp1</span></span>

<span data-ttu-id="d49de-194">Verificare che la libreria di classi dell'interfaccia utente Razor sia in uso:</span><span class="sxs-lookup"><span data-stu-id="d49de-194">Verify the Razor UI class library is in use:</span></span>

* <span data-ttu-id="d49de-195">Passare a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="d49de-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="d49de-196">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="d49de-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="d49de-197">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="d49de-197">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="d49de-198">Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.</span><span class="sxs-lookup"><span data-stu-id="d49de-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="d49de-199">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="d49de-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="d49de-200">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d49de-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="d49de-201">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="d49de-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="d49de-202">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="d49de-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="d49de-203">Layout delle pagine RCL</span><span class="sxs-lookup"><span data-stu-id="d49de-203">RCL Pages layout</span></span>

<span data-ttu-id="d49de-204">Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:</span><span class="sxs-lookup"><span data-stu-id="d49de-204">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="d49de-205">*RazorUIClassLib o delle pagine*</span><span class="sxs-lookup"><span data-stu-id="d49de-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="d49de-206">*RazorUIClassLib/pagine/Shared*</span><span class="sxs-lookup"><span data-stu-id="d49de-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="d49de-207">Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d49de-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="d49de-208">Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="d49de-208">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="d49de-209">Creare un RCL con asset statici</span><span class="sxs-lookup"><span data-stu-id="d49de-209">Create an RCL with static assets</span></span>

<span data-ttu-id="d49de-210">Un RCL può richiedere risorse statiche complementari a cui è possibile fare riferimento dall'app consumer di RCL.</span><span class="sxs-lookup"><span data-stu-id="d49de-210">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="d49de-211">ASP.NET Core consente di creare RCL che includono risorse statiche disponibili per un'app di consumo.</span><span class="sxs-lookup"><span data-stu-id="d49de-211">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="d49de-212">Per includere asset complementari come parte di un RCL, creare una cartella *wwwroot* nella libreria di classi e includere tutti i file necessari in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="d49de-212">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="d49de-213">Quando si esegue la compressione di un RCL, tutti gli asset complementari nella cartella *wwwroot* vengono inclusi automaticamente nel pacchetto.</span><span class="sxs-lookup"><span data-stu-id="d49de-213">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="d49de-214">Escludi asset statici</span><span class="sxs-lookup"><span data-stu-id="d49de-214">Exclude static assets</span></span>

<span data-ttu-id="d49de-215">Per escludere asset statici, aggiungere il percorso di esclusione desiderato `$(DefaultItemExcludes)` al gruppo di proprietà nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="d49de-215">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="d49de-216">Separare le voci con un punto`;`e virgola ().</span><span class="sxs-lookup"><span data-stu-id="d49de-216">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="d49de-217">Nell'esempio seguente, il foglio di stile *lib. CSS* nella cartella *wwwroot* non è considerato un asset statico e non è incluso nel RCL pubblicato:</span><span class="sxs-lookup"><span data-stu-id="d49de-217">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="d49de-218">Integrazione typescript</span><span class="sxs-lookup"><span data-stu-id="d49de-218">Typescript integration</span></span>

<span data-ttu-id="d49de-219">Per includere i file TypeScript in un RCL:</span><span class="sxs-lookup"><span data-stu-id="d49de-219">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="d49de-220">Inserire i file TypeScript ( *. TS*) all'esterno della cartella *wwwroot*</span><span class="sxs-lookup"><span data-stu-id="d49de-220">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="d49de-221">Inserire, ad esempio, i file in una cartella *client* .</span><span class="sxs-lookup"><span data-stu-id="d49de-221">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="d49de-222">Configurare l'output di compilazione TypeScript per la cartella *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="d49de-222">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="d49de-223">Impostare la `TypescriptOutDir` proprietà all'interno di `PropertyGroup` un oggetto nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="d49de-223">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="d49de-224">Includere la destinazione typescript come dipendenza della `ResolveCurrentProjectStaticWebAssets` destinazione aggiungendo la destinazione seguente all'interno di un oggetto `PropertyGroup` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="d49de-224">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="d49de-225">Utilizzare il contenuto da un RCL a cui si fa riferimento</span><span class="sxs-lookup"><span data-stu-id="d49de-225">Consume content from a referenced RCL</span></span>

<span data-ttu-id="d49de-226">I file inclusi nella cartella *wwwroot* di RCL vengono esposti all'app consumer sotto il prefisso `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="d49de-226">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="d49de-227">Ad esempio, una libreria denominata *Razor. Class. lib* genera un percorso al contenuto statico in `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="d49de-227">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="d49de-228">L'app consumer fa riferimento a risorse statiche fornite dalla libreria `<script>`con `<style>` `<img>`,, e altri tag HTML.</span><span class="sxs-lookup"><span data-stu-id="d49de-228">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="d49de-229">Per l'app consumer deve essere abilitato il [supporto file statico](xref:fundamentals/static-files) in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="d49de-229">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="d49de-230">Quando si esegue l'app consumer dall'output di`dotnet run`compilazione (), le risorse Web statiche sono abilitate per impostazione predefinita nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d49de-230">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="d49de-231">Per supportare asset in altri ambienti durante l'esecuzione dall'output di compilazione `UseStaticWebAssets` , chiamare sul generatore host in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d49de-231">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="d49de-232">Quando `UseStaticWebAssets` si esegue un'app dall'output pubblicato (`dotnet publish`), la chiamata non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="d49de-232">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="d49de-233">Flusso di sviluppo per più progetti</span><span class="sxs-lookup"><span data-stu-id="d49de-233">Multi-project development flow</span></span>

<span data-ttu-id="d49de-234">Quando viene eseguita l'app consumer:</span><span class="sxs-lookup"><span data-stu-id="d49de-234">When the consuming app runs:</span></span>

* <span data-ttu-id="d49de-235">Gli asset nel RCL rimanere nelle cartelle originali.</span><span class="sxs-lookup"><span data-stu-id="d49de-235">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="d49de-236">Gli asset non vengono spostati nell'app consumer.</span><span class="sxs-lookup"><span data-stu-id="d49de-236">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="d49de-237">Tutte le modifiche apportate all'interno della cartella *wwwroot* di RCL vengono riflesse nell'app consumer dopo che il RCL viene ricompilato e senza ricompilare l'app consumer.</span><span class="sxs-lookup"><span data-stu-id="d49de-237">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="d49de-238">Quando viene compilato il RCL, viene generato un manifesto che descrive i percorsi di asset Web statici.</span><span class="sxs-lookup"><span data-stu-id="d49de-238">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="d49de-239">L'app consumer legge il manifesto in fase di esecuzione per usare gli asset dei progetti e dei pacchetti a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="d49de-239">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="d49de-240">Quando si aggiunge un nuovo asset a un RCL, è necessario ricompilare il RCL per aggiornare il manifesto prima che un'app che utilizza possa accedere al nuovo asset.</span><span class="sxs-lookup"><span data-stu-id="d49de-240">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="d49de-241">Pubblica</span><span class="sxs-lookup"><span data-stu-id="d49de-241">Publish</span></span>

<span data-ttu-id="d49de-242">Quando viene pubblicata l'app, gli asset complementari di tutti i progetti e i pacchetti a cui viene fatto riferimento vengono copiati nella cartella wwwroot `_content/{LIBRARY NAME}/`dell'app pubblicata in.</span><span class="sxs-lookup"><span data-stu-id="d49de-242">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end
