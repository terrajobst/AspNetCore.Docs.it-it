---
title: Visual Studio profili di pubblicazione per la distribuzione di app ASP.NET Core
author: rick-anderson
description: Informazioni su come creare profili di pubblicazione per le applicazioni ASP.NET Core in Visual Studio.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="25f20-103">Visual Studio profili di pubblicazione per la distribuzione di app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25f20-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="25f20-104">Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25f20-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25f20-105">Questo articolo tratta dell'uso di Visual Studio 2017 per creare profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="25f20-106">I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="25f20-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="25f20-107">L'articolo illustra i dettagli del processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="25f20-108">Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="25f20-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="25f20-109">Il file *.csproj* seguente è stato creato con il comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="25f20-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="25f20-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="25f20-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="25f20-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="25f20-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="25f20-112">L'attributo `Sdk` nell'elemento `<Project>` (nella prima riga) del markup riportato sopra esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="25f20-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="25f20-113">Importa il file delle proprietà da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.</span><span class="sxs-lookup"><span data-stu-id="25f20-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="25f20-114">Importa il file di destinazioni da  *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.</span><span class="sxs-lookup"><span data-stu-id="25f20-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="25f20-115">Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="25f20-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="25f20-116">`Microsoft.NET.Sdk.Web` dipende da:</span><span class="sxs-lookup"><span data-stu-id="25f20-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="25f20-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="25f20-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="25f20-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="25f20-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="25f20-119">Che genera le seguenti proprietà e le destinazioni da importare:</span><span class="sxs-lookup"><span data-stu-id="25f20-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="25f20-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="25f20-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="25f20-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="25f20-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="25f20-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="25f20-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="25f20-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="25f20-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="25f20-124">Pubblicare l'importazione delle destinazioni destra set di destinazioni in base al metodo di pubblicazione utilizzato.</span><span class="sxs-lookup"><span data-stu-id="25f20-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="25f20-125">Quando MSBuild o Visual Studio carica un progetto, vengono eseguite le seguenti azioni rilevanti:</span><span class="sxs-lookup"><span data-stu-id="25f20-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="25f20-126">Compilare un progetto</span><span class="sxs-lookup"><span data-stu-id="25f20-126">Build project</span></span>
* <span data-ttu-id="25f20-127">Calcolare i file da pubblicare</span><span class="sxs-lookup"><span data-stu-id="25f20-127">Compute files to publish</span></span>
* <span data-ttu-id="25f20-128">Pubblicare i file nella destinazione</span><span class="sxs-lookup"><span data-stu-id="25f20-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="25f20-129">Calcolare gli elementi del progetto</span><span class="sxs-lookup"><span data-stu-id="25f20-129">Compute project items</span></span>

<span data-ttu-id="25f20-130">Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file).</span><span class="sxs-lookup"><span data-stu-id="25f20-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="25f20-131">L'attributo `item type` determina la modalità di elaborazione del file.</span><span class="sxs-lookup"><span data-stu-id="25f20-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="25f20-132">Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`.</span><span class="sxs-lookup"><span data-stu-id="25f20-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="25f20-133">I file presenti nell'elenco di elementi `Compile` vengono compilati.</span><span class="sxs-lookup"><span data-stu-id="25f20-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="25f20-134">Il `Content` elenco di elementi contiene i file che vengono pubblicati oltre gli output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="25f20-135">Per impostazione predefinita, i file corrispondenti al criterio `wwwroot/**` sono inclusi nel `Content` elemento.</span><span class="sxs-lookup"><span data-stu-id="25f20-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="25f20-136">[wwwroot /\* \* è un modello il glob](https://gruntjs.com/configuring-tasks#globbing-patterns) che specifica tutti i file di *wwwroot* cartella **e** le sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="25f20-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="25f20-137">Per aggiungere esplicitamente un file all'elenco di pubblicazione, aggiungere il file direttamente nel file *.csproj*, come illustrato in [Inclusione di file](#including-files).</span><span class="sxs-lookup"><span data-stu-id="25f20-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="25f20-138">Quando si seleziona il **pubblica** button in Visual Studio o quando si pubblicano dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="25f20-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="25f20-139">Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).</span><span class="sxs-lookup"><span data-stu-id="25f20-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="25f20-140">Solo Visual Studio: i pacchetti NuGet vengono ripristinati.</span><span class="sxs-lookup"><span data-stu-id="25f20-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="25f20-141">(Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)</span><span class="sxs-lookup"><span data-stu-id="25f20-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="25f20-142">Il progetto viene compilato.</span><span class="sxs-lookup"><span data-stu-id="25f20-142">The project builds.</span></span>
* <span data-ttu-id="25f20-143">Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).</span><span class="sxs-lookup"><span data-stu-id="25f20-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="25f20-144">Il progetto viene pubblicato.</span><span class="sxs-lookup"><span data-stu-id="25f20-144">The project is published.</span></span> <span data-ttu-id="25f20-145">(I file calcolati vengono copiati nella destinazione di pubblicazione.)</span><span class="sxs-lookup"><span data-stu-id="25f20-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="25f20-146">Quando fa riferimento a un progetto ASP.NET Core `Microsoft.NET.Sdk.Web` nel file di progetto, un *app_offline.htm* file si trova nella radice della directory dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="25f20-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="25f20-147">Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="25f20-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="25f20-148">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="25f20-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="25f20-149">Pubblicazione della riga di comando di base</span><span class="sxs-lookup"><span data-stu-id="25f20-149">Basic command-line publishing</span></span>

<span data-ttu-id="25f20-150">La pubblicazione della riga di comando funziona in tutte le piattaforme .NET Core è supportato e non richiede Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25f20-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="25f20-151">Negli esempi seguenti, il comando `dotnet publish` viene eseguito dalla directory del progetto (che contiene il file *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="25f20-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="25f20-152">Se non è nella cartella del progetto, passare in modo esplicito il percorso del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="25f20-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="25f20-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="25f20-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="25f20-154">Eseguire i comandi seguenti per creare e pubblicare un'app Web:</span><span class="sxs-lookup"><span data-stu-id="25f20-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="25f20-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="25f20-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="25f20-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="25f20-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="25f20-157">`dotnet publish` produce un output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="25f20-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="25f20-158">La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="25f20-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="25f20-159">Il valore predefinito per `$(Configuration)` è Debug.</span><span class="sxs-lookup"><span data-stu-id="25f20-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="25f20-160">Nel campione precedente, il `<TargetFramework>` è `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="25f20-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="25f20-161">`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="25f20-162">Il comando seguente specifica un build `Release` e la directory di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="25f20-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="25f20-163">Il `dotnet publish` comando chiama MSBuild che richiama la `Publish` destinazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="25f20-164">I parametri passati a `dotnet publish` vengono passati a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="25f20-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="25f20-165">Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="25f20-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="25f20-166">Il parametro `-o` esegue il mapping a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="25f20-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="25f20-167">Proprietà di MSBuild può essere passata tramite uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="25f20-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="25f20-168">Il comando seguente pubblica un build `Release` a una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="25f20-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="25f20-169">La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="25f20-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="25f20-170">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="25f20-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="25f20-171">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="25f20-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="25f20-172">La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.</span><span class="sxs-lookup"><span data-stu-id="25f20-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="25f20-173">Profili di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="25f20-173">Publish profiles</span></span>

<span data-ttu-id="25f20-174">Questa sezione usa Visual Studio 2017 e versioni successive per creare profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="25f20-175">Una volta creata, la pubblicazione da Visual Studio o la riga di comando è disponibile.</span><span class="sxs-lookup"><span data-stu-id="25f20-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="25f20-176">I profili di pubblicazione possono semplificare il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="25f20-177">Più profili di pubblicazione può essere presente.</span><span class="sxs-lookup"><span data-stu-id="25f20-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="25f20-178">Per creare un profilo di pubblicazione in Visual Studio, fare clic sul progetto in Esplora soluzioni e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="25f20-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="25f20-179">In alternativa, selezionare **pubblica \<nome progetto >** dal menu Compila.</span><span class="sxs-lookup"><span data-stu-id="25f20-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="25f20-180">Viene visualizzata la scheda **Pubblica** della pagina di capacità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="25f20-181">Se il progetto non contiene un profilo di pubblicazione, viene visualizzata la pagina seguente:</span><span class="sxs-lookup"><span data-stu-id="25f20-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Seleziona la scheda pubblica della pagina di capacità dell'applicazione con Azure, IIS, FTB, la cartella con Azure.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="25f20-184">Selezionando **Cartella**, il pulsante **Pubblica** crea un profilo di pubblicazione della cartella ed esegue la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza la cartella Azure, IIS, FTB](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="25f20-186">Dopo aver creato un profilo di pubblicazione, il **pubblica** scheda modifiche, quindi selezionare **Crea nuovo profilo** per creare un nuovo profilo.</span><span class="sxs-lookup"><span data-stu-id="25f20-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza il FolderProfile creato nell'ultimo passaggio e il collegamento Crea nuovo profilo](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="25f20-188">La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="25f20-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="25f20-189">Servizio app di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="25f20-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="25f20-190">IIS, FTP e così via (per qualunque server Web)</span><span class="sxs-lookup"><span data-stu-id="25f20-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="25f20-191">Cartella</span><span class="sxs-lookup"><span data-stu-id="25f20-191">Folder</span></span>
* <span data-ttu-id="25f20-192">Importa profilo (consente di importare il profilo).</span><span class="sxs-lookup"><span data-stu-id="25f20-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="25f20-193">Macchine virtuali di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="25f20-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="25f20-194">Per altre informazioni vedere [Quali sono le opzioni di pubblicazione più adatte?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="25f20-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="25f20-195">Quando si crea un profilo di pubblicazione con Visual Studio, un *proprietà/PublishProfiles/\<nome pubblicazione > pubxml* viene creato il file MSBuild.</span><span class="sxs-lookup"><span data-stu-id="25f20-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="25f20-196">Questo file *.pubxml* è un file MSBuild e contiene le impostazioni di configurazione della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="25f20-197">Questo file può essere modificato per personalizzare la compilazione e processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="25f20-198">Questo file viene letto dal processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-198">This file is read by the publishing process.</span></span> <span data-ttu-id="25f20-199">`<LastUsedBuildConfiguration>`è uno speciale perché è una proprietà globale e non deve essere in qualsiasi file che viene importato nella compilazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="25f20-200">Per altre informazioni vedere [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: come impostare la proprietà di configurazione).</span><span class="sxs-lookup"><span data-stu-id="25f20-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="25f20-201">Il *pubxml* file non deve essere verificato nel controllo del codice sorgente perché dipende il *User* file.</span><span class="sxs-lookup"><span data-stu-id="25f20-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="25f20-202">Il file *.user* non deve mai essere selezionato nel controllo del codice sorgente perché può contenere informazioni riservate ed è valido solo per un unico utente e computer.</span><span class="sxs-lookup"><span data-stu-id="25f20-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="25f20-203">Le informazioni riservate (come la password di pubblicazione) sono crittografate a livello di ogni utente/computer e archiviate nel file *Properties/PublishProfiles/\<publish name>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="25f20-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="25f20-204">Poiché questo file può contenere informazioni riservate, **non** deve essere selezionato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="25f20-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="25f20-205">Per una panoramica su come pubblicare un'app web in ASP.NET Core vedere [Host e distribuire](index.md).</span><span class="sxs-lookup"><span data-stu-id="25f20-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="25f20-206">[Host e distribuire](index.md) è un progetto open source in https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="25f20-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="25f20-207">`dotnet publish`cartella, MSDeploy, può essere usata e [KUDU](https://github.com/projectkudu/kudu/wiki) profili di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="25f20-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="25f20-208">Cartella (e multipiattaforma):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="25f20-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="25f20-209">MSDeploy (attualmente questo funziona solo in windows poiché MSDeploy non multipiattaforma):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="25f20-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="25f20-210">Pacchetto di MSDeploy (attualmente questo funziona solo in windows poiché MSDeploy non multipiattaforma):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="25f20-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="25f20-211">Negli esempi precedenti, **non** passare `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="25f20-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="25f20-212">Per ulteriori informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="25f20-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="25f20-213">`dotnet publish` supporta le API KUDU per la pubblicazione in Azure da qualsiasi piattaforma.</span><span class="sxs-lookup"><span data-stu-id="25f20-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="25f20-214">Visual Studio la pubblicazione supporta le API KUDU ma è supportato dal websdk per plat cross pubblicare in Azure.</span><span class="sxs-lookup"><span data-stu-id="25f20-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="25f20-215">Aggiungere un profilo di pubblicazione alla cartella *Properties/PublishProfiles* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="25f20-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="25f20-216">Eseguire il comando seguente segnala immediatamente il backup del contenuto di pubblicazione e pubblicarla in Azure utilizzando le API KUDU:</span><span class="sxs-lookup"><span data-stu-id="25f20-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="25f20-217">Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:</span><span class="sxs-lookup"><span data-stu-id="25f20-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="25f20-218">Durante la pubblicazione con un profilo denominato *FolderProfile*, è possibile eseguire uno dei comandi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="25f20-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="25f20-219">Quando si richiama `dotnet build`, chiama `msbuild` per eseguire la compilazione e processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="25f20-220">La chiamata `dotnet build` o `msbuild` è essenzialmente equivalente durante il passaggio in un profilo di cartella.</span><span class="sxs-lookup"><span data-stu-id="25f20-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="25f20-221">Quando si chiama MSBuild direttamente in Windows, viene utilizzata la versione di .NET Framework di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="25f20-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="25f20-222">MSDeploy è attualmente limitato ai computer Windows per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="25f20-223">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle.</span><span class="sxs-lookup"><span data-stu-id="25f20-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="25f20-224">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows).</span><span class="sxs-lookup"><span data-stu-id="25f20-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="25f20-225">Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.</span><span class="sxs-lookup"><span data-stu-id="25f20-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="25f20-226">Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="25f20-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="25f20-227">Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`.</span><span class="sxs-lookup"><span data-stu-id="25f20-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="25f20-228">Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="25f20-229">Il `<LastUsedBuildConfiguration>` proprietà di configurazione è speciale e non deve essere sottoposto a override in un file di MSBuild importato.</span><span class="sxs-lookup"><span data-stu-id="25f20-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="25f20-230">Questa proprietà può essere sottoposto a override dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="25f20-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="25f20-231">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="25f20-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="25f20-232">Mediante MSBuild:</span><span class="sxs-lookup"><span data-stu-id="25f20-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="25f20-233">Pubblicare in un endpoint di MSDeploy dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="25f20-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="25f20-234">Come menzionato in precedenza, la pubblicazione può essere eseguita tramite `dotnet publish` o `msbuild` comando.</span><span class="sxs-lookup"><span data-stu-id="25f20-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="25f20-235">`dotnet publish` viene eseguito nel contesto di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="25f20-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="25f20-236">Il `msbuild` comando richiede .NET framework e pertanto è limitato agli ambienti di Windows.</span><span class="sxs-lookup"><span data-stu-id="25f20-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="25f20-237">Il modo più semplice per pubblicare con MSDeploy è quello di creare prima un profilo di pubblicazione in Visual Studio 2017, quindi usare il profilo dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="25f20-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="25f20-238">Nell'esempio seguente viene creata un'app web ASP.NET Core (utilizzando `dotnet new mvc`), e viene aggiunto un profilo di pubblicazione di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25f20-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="25f20-239">Eseguire `msbuild` da un **prompt dei comandi per sviluppatori per Visual Studio 2017**.</span><span class="sxs-lookup"><span data-stu-id="25f20-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="25f20-240">Prompt dei comandi per sviluppatori è corrette *msbuild.exe* nel percorso con un set di variabili MSBuild.</span><span class="sxs-lookup"><span data-stu-id="25f20-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="25f20-241">MSBuild usa la sintassi seguente:</span><span class="sxs-lookup"><span data-stu-id="25f20-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="25f20-242">Ottenere il `Password` dal  *\<nome pubblicazione >. PublishSettings* file.</span><span class="sxs-lookup"><span data-stu-id="25f20-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="25f20-243">Scaricare il *. PublishSettings* file da uno:</span><span class="sxs-lookup"><span data-stu-id="25f20-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="25f20-244">Esplora soluzioni: Nell'App Web e scegliere **Scarica profilo di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="25f20-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="25f20-245">Il portale di gestione di Azure: Selezionare **profilo di pubblicazione Get** nel pannello App Web.</span><span class="sxs-lookup"><span data-stu-id="25f20-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="25f20-246">Lo `Username` può essere trovato nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="25f20-247">Nel campione seguente viene usato il profilo di pubblicazione "Web11112 - Distribuzione Web":</span><span class="sxs-lookup"><span data-stu-id="25f20-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="25f20-248">Esclusione di file</span><span class="sxs-lookup"><span data-stu-id="25f20-248">Excluding files</span></span>

<span data-ttu-id="25f20-249">Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="25f20-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="25f20-250">`msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="25f20-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="25f20-251">Seguente, ad esempio, `<Content>` markup elemento esclude tutto il testo (*. txt*) i file dal *wwwroot/content* cartella e le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="25f20-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="25f20-252">Il markup riportato sopra può essere aggiunto a un profilo di pubblicazione o al file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="25f20-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="25f20-253">Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="25f20-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="25f20-254">Il markup dell'elemento `<MsDeploySkipRules>` seguente esclude tutti i file dalla cartella *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="25f20-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="25f20-255">`<MsDeploySkipRules>`non verrà eliminato il *ignorare* destinazioni dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="25f20-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="25f20-256">`<Content>`cartelle e file di destinazione vengono eliminate dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="25f20-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="25f20-257">Si supponga, ad esempio, che un'app web distribuite ha i seguenti file:</span><span class="sxs-lookup"><span data-stu-id="25f20-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="25f20-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="25f20-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="25f20-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="25f20-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="25f20-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="25f20-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="25f20-261">Se le operazioni seguenti `<MsDeploySkipRules>` markup viene aggiunto, non è possibile eliminare tali file nel sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="25f20-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="25f20-262">Il `<MsDeploySkipRules>` markup illustrato in precedenza impedisce il *ignorata* file vengano depoyed ma non elimina i file vengano distribuite.</span><span class="sxs-lookup"><span data-stu-id="25f20-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="25f20-263">Le operazioni seguenti `<Content>` markup Elimina i file di destinazione nel sito di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="25f20-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="25f20-264">Utilizzando la distribuzione della riga di comando con il `<Content>` markup sopra produce output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="25f20-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a><span data-ttu-id="25f20-265">Inclusione di file</span><span class="sxs-lookup"><span data-stu-id="25f20-265">Including files</span></span>

<span data-ttu-id="25f20-266">Il markup seguente include un *immagini* cartella all'esterno della directory di progetto per il *wwwroot/immagini* cartella del sito pubblica:</span><span class="sxs-lookup"><span data-stu-id="25f20-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="25f20-267">Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="25f20-268">Se viene aggiunto il *csproj* file, è incluso in ogni profilo di pubblicazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="25f20-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="25f20-269">Il markup evidenziato seguente illustra come:</span><span class="sxs-lookup"><span data-stu-id="25f20-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="25f20-270">Copiare un file dall'esterno del progetto nella cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="25f20-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="25f20-271">Escludere la cartella *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="25f20-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="25f20-272">Escludere *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25f20-272">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="25f20-273">Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="25f20-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="25f20-274">Eseguire una destinazione prima o dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="25f20-274">Run a target before or after publishing</span></span>

<span data-ttu-id="25f20-275">L'elemento predefinito `BeforePublish` e `AfterPublish` destinazioni possono essere utilizzate per eseguire una destinazione, prima o dopo la destinazione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="25f20-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="25f20-276">È possibile aggiungere il markup seguente al profilo di pubblicazione per registrare messaggi nell'output di console prima e dopo la pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="25f20-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="25f20-277">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="25f20-277">The Kudu service</span></span>

<span data-ttu-id="25f20-278">Per visualizzare i file di una distribuzione di app web di servizio app di Azure, utilizzare il [servizio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="25f20-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="25f20-279">Aggiungere il `scm` token per il nome dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="25f20-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="25f20-280">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="25f20-280">For example:</span></span>

| <span data-ttu-id="25f20-281">URL</span><span class="sxs-lookup"><span data-stu-id="25f20-281">URL</span></span>                                    | <span data-ttu-id="25f20-282">Risultato</span><span class="sxs-lookup"><span data-stu-id="25f20-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="25f20-283">App Web</span><span class="sxs-lookup"><span data-stu-id="25f20-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="25f20-284">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="25f20-284">Kudu sevice</span></span> |

<span data-ttu-id="25f20-285">Selezionare la voce di menu [Console di debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) per visualizzare/modificare/eliminare/aggiungere file.</span><span class="sxs-lookup"><span data-stu-id="25f20-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25f20-286">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="25f20-286">Additional resources</span></span>

* <span data-ttu-id="25f20-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) semplifica la distribuzione di App web e siti Web ai server IIS.</span><span class="sxs-lookup"><span data-stu-id="25f20-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="25f20-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemi di file e funzionalità di richiesta per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="25f20-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
