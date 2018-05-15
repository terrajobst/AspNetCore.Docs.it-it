---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: In questo articolo viene illustrato come creare un'interfaccia utente Razor riutilizzabile in una libreria di classi.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="0cee4-103">Creare un'interfaccia utente riutilizzabile tramite il progetto di libreria di classi Razor in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0cee4-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="0cee4-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0cee4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0cee4-105">È possibile compilare in una libreria di classi Razor visualizzazioni, pagine, controller, modelli di pagine e modelli di dati Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="0cee4-106">La libreria di classi Razor può essere inclusa nel pacchetto e riutilizzata.</span><span class="sxs-lookup"><span data-stu-id="0cee4-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="0cee4-107">Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute.</span><span class="sxs-lookup"><span data-stu-id="0cee4-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="0cee4-108">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="0cee4-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="0cee4-109">Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="0cee4-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="0cee4-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0cee4-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="0cee4-111">Creare una libreria di classi contenente l'interfaccia utente Razor</span><span class="sxs-lookup"><span data-stu-id="0cee4-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cee4-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cee4-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0cee4-113">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0cee4-114">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0cee4-115">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="0cee4-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="0cee4-116">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0cee4-117">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0cee4-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0cee4-118">Eseguire `dotnet new razorclasslib` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="0cee4-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="0cee4-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0cee4-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="0cee4-120">Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0cee4-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="0cee4-121">Aggiungere i file Razor alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="0cee4-122">È consigliabile che il contenuto della libreria di classi Razor venga inserito nella cartella *Aree*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="0cee4-123">Riferimento ai contenuti della libreria di classi Razor</span><span class="sxs-lookup"><span data-stu-id="0cee4-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="0cee4-124">Il riferimento alla libreria di classi Razor può essere eseguito da:</span><span class="sxs-lookup"><span data-stu-id="0cee4-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="0cee4-125">Pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="0cee4-125">NuGet package.</span></span> <span data-ttu-id="0cee4-126">Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="0cee4-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="0cee4-127">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="0cee4-128">Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="0cee4-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="0cee4-129">Accesso ai file parziali nella libreria di classi Razor</span><span class="sxs-lookup"><span data-stu-id="0cee4-129">Partial files access in the RCL</span></span>

<span data-ttu-id="0cee4-130">Per il contenuto al di fuori della libreria di classi Razor, il runtime di ASP.NET Core non esegue la ricerca di file parziali nella libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="0cee4-131">Ad esempio, nel download di esempio, il riferimento alla visualizzazione parziale *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* **non** può essere eseguito in *WebApp1\Pages\About.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="0cee4-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="0cee4-132">Le pagine nella libreria di classi Razor ( *RazorUIClassLib /* **possono** tuttavia accedere a *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="0cee4-133">Procedura dettagliata: Creare un progetto libreria di classi Razor e usarlo da un progetto di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0cee4-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="0cee4-134">È possibile scaricare il [progetto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) e testarlo anziché crearlo.</span><span class="sxs-lookup"><span data-stu-id="0cee4-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="0cee4-135">Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto.</span><span class="sxs-lookup"><span data-stu-id="0cee4-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="0cee4-136">È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="0cee4-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="0cee4-137">Testare l'app di download</span><span class="sxs-lookup"><span data-stu-id="0cee4-137">Test the download app</span></span>

<span data-ttu-id="0cee4-138">Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="0cee4-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cee4-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cee4-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0cee4-140">Aprire il file con estensione *sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0cee4-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="0cee4-141">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="0cee4-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0cee4-142">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0cee4-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0cee4-143">Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.</span><span class="sxs-lookup"><span data-stu-id="0cee4-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="0cee4-144">Passare alla directory *WebApp1* ed eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="0cee4-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="0cee4-145">Seguire le istruzioni indicate in [Testare WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="0cee4-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="0cee4-146">Creare una libreria di classi Razor</span><span class="sxs-lookup"><span data-stu-id="0cee4-146">Create a Razor Class Library</span></span>

<span data-ttu-id="0cee4-147">In questa sezione viene creata una libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="0cee4-148">I file Razor vengono aggiunti alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cee4-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cee4-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0cee4-150">Creare il progetto di libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="0cee4-150">Create the RCL project:</span></span>

* <span data-ttu-id="0cee4-151">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0cee4-152">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0cee4-153">Denominare l'app **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="0cee4-154">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="0cee4-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="0cee4-155">Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="0cee4-156">Creare l'app Web di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="0cee4-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="0cee4-157">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >  **Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="0cee4-158">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0cee4-159">Denominare l'app **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="0cee4-160">Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="0cee4-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="0cee4-161">Selezionare **Applicazione Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="0cee4-162">Aggiungere i file e le cartelle Razor al progetto.</span><span class="sxs-lookup"><span data-stu-id="0cee4-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="0cee4-163">Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="0cee4-164">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0cee4-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="0cee4-165">Copiare il file *_ViewStart.cshtml* dal progetto WebApp1 in *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="0cee4-166">Il file [viewstart](xref:mvc/views/layout#running-code-before-each-view) è necessario per usare il layout del progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0cee4-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0cee4-167">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0cee4-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0cee4-168">Eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="0cee4-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="0cee4-169">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="0cee4-169">The preceding commands:</span></span>

* <span data-ttu-id="0cee4-170">Crea la libreria di classi Razor `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="0cee4-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="0cee4-171">Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="0cee4-172">Il parametro `-np` crea la pagina senza un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="0cee4-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="0cee4-173">Crea un file [viewstart](xref:mvc/views/layout#running-code-before-each-view) e lo aggiunge alla libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="0cee4-174">Il file viewstart è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione seguente).</span><span class="sxs-lookup"><span data-stu-id="0cee4-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="0cee4-175">Aggiornare Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="0cee4-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="0cee4-176">Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0cee4-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="0cee4-177">Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0cee4-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="0cee4-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="0cee4-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="0cee4-179">Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="0cee4-180">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0cee4-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="0cee4-181">Per altre informazioni su viewimports, vedere [Importazione delle direttive condivise](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="0cee4-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="0cee4-182">Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:</span><span class="sxs-lookup"><span data-stu-id="0cee4-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="0cee4-183">L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="0cee4-184">*RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.</span><span class="sxs-lookup"><span data-stu-id="0cee4-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="0cee4-185">Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0cee4-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0cee4-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0cee4-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0cee4-187">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="0cee4-188">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Dipendenze di compilazione** > **Dipendenze progetto**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="0cee4-189">Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="0cee4-190">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e scegliere **Aggiungi** > **Riferimento**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="0cee4-191">Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cee4-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="0cee4-192">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="0cee4-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0cee4-193">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0cee4-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0cee4-194">Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e la libreria di classi Razor:</span><span class="sxs-lookup"><span data-stu-id="0cee4-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="0cee4-195">Compilare ed eseguire l'app Web:</span><span class="sxs-lookup"><span data-stu-id="0cee4-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="0cee4-196">Testare WebApp1</span><span class="sxs-lookup"><span data-stu-id="0cee4-196">Test WebApp1</span></span>

<span data-ttu-id="0cee4-197">Verificare che la libreria di classi dell'interfaccia utente Razor sia in uso.</span><span class="sxs-lookup"><span data-stu-id="0cee4-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="0cee4-198">Passare a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="0cee4-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="0cee4-199">Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine</span><span class="sxs-lookup"><span data-stu-id="0cee4-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="0cee4-200">Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="0cee4-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="0cee4-201">Ad esempio, aggiungere *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1 e Page1 in WebApp1 avrà la precedenza su Page1 nella libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="0cee4-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="0cee4-202">Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.</span><span class="sxs-lookup"><span data-stu-id="0cee4-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="0cee4-203">Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cee4-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="0cee4-204">Aggiornare il markup per indicare la nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="0cee4-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="0cee4-205">Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.</span><span class="sxs-lookup"><span data-stu-id="0cee4-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
