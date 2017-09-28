---
title: Creare profili di pubblicazione per Visual Studio e MSBuild
author: rick-anderson
description: Illustra la pubblicazione sul Web in Visual Studio.
keywords: ASP.NET Core, pubblicazione sul Web, pubblicazione, msbuild, distribuzione Web, pubblicazione dotnet, Visual Studio 2017
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 665c98b5ac16bb9739af4ac204fca59a55dbb812
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="fbf3d-104">Creare profili di pubblicazione per Visual Studio e MSBuild, per distribuire applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbf3d-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="fbf3d-105">Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fbf3d-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fbf3d-106">Questo articolo tratta dell'uso di Visual Studio 2017 per creare profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="fbf3d-107">I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span>

<span data-ttu-id="fbf3d-108">Il file *.csproj* seguente è stato creato con il comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-108">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fbf3d-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fbf3d-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fbf3d-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fbf3d-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="fbf3d-111">L'attributo `Sdk` nell'elemento `<Project>` (nella prima riga) del markup riportato sopra esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-111">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="fbf3d-112">Importa il file `props` da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-112">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="fbf3d-113">Importa il file di destinazioni da * $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="fbf3d-114">Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="fbf3d-115">`Microsoft.NET.Sdk.Web` dipende da:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-115">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="fbf3d-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="fbf3d-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="fbf3d-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="fbf3d-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="fbf3d-118">Che determina l'importazione dei `props` e `targets` seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-118">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="fbf3d-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="fbf3d-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="fbf3d-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="fbf3d-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="fbf3d-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="fbf3d-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="fbf3d-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="fbf3d-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="fbf3d-123">Le destinazioni di pubblicazione importeranno nuovamente il set corretto di destinazioni in base al metodo di pubblicazione usato.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-123">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="fbf3d-124">Quando MSBuild o Visual Studio carica un progetto, vengono eseguite le seguenti azioni rilevanti:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-124">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="fbf3d-125">Compilare un progetto</span><span class="sxs-lookup"><span data-stu-id="fbf3d-125">Build project</span></span>
* <span data-ttu-id="fbf3d-126">Calcolare i file da pubblicare</span><span class="sxs-lookup"><span data-stu-id="fbf3d-126">Compute files to publish</span></span>
* <span data-ttu-id="fbf3d-127">Pubblicare i file nella destinazione</span><span class="sxs-lookup"><span data-stu-id="fbf3d-127">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="fbf3d-128">Calcolare gli elementi del progetto</span><span class="sxs-lookup"><span data-stu-id="fbf3d-128">Compute project items</span></span>

<span data-ttu-id="fbf3d-129">Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="fbf3d-130">L'attributo `item type` determina la modalità di elaborazione del file.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="fbf3d-131">Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="fbf3d-132">I file presenti nell'elenco di elementi `Compile` vengono compilati.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="fbf3d-133">L'elenco di elementi `Content` contiene i file che saranno pubblicati in aggiunta agli output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-133">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="fbf3d-134">Per impostazione predefinita, i file corrispondenti al criterio wwwroot/** saranno inclusi nell'elemento `Content`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-134">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="fbf3d-135">[wwwroot/** è un criterio GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns) che specifica tutti i file nella cartella *wwwroot* **e** relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-135">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="fbf3d-136">Se è necessario aggiungere esplicitamente un file all'elenco di pubblicazione è possibile aggiungere il file direttamente nel file *.csproj*, come illustrato in [Inclusione di file](#including-files).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-136">If you need to explicitly add a file to the publish list you can add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="fbf3d-137">Quando si seleziona il pulsante **Pubblica** in Visual Studio o quando si pubblica dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-137">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="fbf3d-138">Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-138">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="fbf3d-139">Solo Visual Studio: i pacchetti NuGet vengono ripristinati.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-139">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="fbf3d-140">(Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)</span><span class="sxs-lookup"><span data-stu-id="fbf3d-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="fbf3d-141">Il progetto viene compilato.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-141">The project builds.</span></span>
- <span data-ttu-id="fbf3d-142">Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-142">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="fbf3d-143">Il progetto viene pubblicato.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-143">The project is published.</span></span> <span data-ttu-id="fbf3d-144">(I file calcolati vengono copiati nella destinazione di pubblicazione.)</span><span class="sxs-lookup"><span data-stu-id="fbf3d-144">(The computed files are copied to the publish destination.)</span></span>

## <a name="simple-command-line-publishing"></a><span data-ttu-id="fbf3d-145">Pubblicazione semplice dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="fbf3d-145">Simple command line publishing</span></span>

<span data-ttu-id="fbf3d-146">Questa sezione funziona su tutte le piattaforme .NET Core supportate e non richiede Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-146">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="fbf3d-147">Negli esempi seguenti, il comando `dotnet publish` viene eseguito dalla directory del progetto (che contiene il file *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-147">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="fbf3d-148">Se non si è nella cartella del progetto, è possibile passare esplicitamente nel percorso del file del progetto.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-148">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="fbf3d-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-149">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="fbf3d-150">Eseguire i comandi seguenti per creare e pubblicare un'app Web:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-150">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet restore
dotnet publish
```

<span data-ttu-id="fbf3d-151">`dotnet publish` produce un output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-151">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

<span data-ttu-id="fbf3d-152">La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="fbf3d-153">Il valore predefinito per `$(Configuration)` è Debug.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-153">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="fbf3d-154">Nel campione precedente, il `<TargetFramework>` è `netcoreapp1.1`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-154">In the sample above, the `<TargetFramework>` is `netcoreapp1.1`.</span></span> <span data-ttu-id="fbf3d-155">Il percorso effettivo nel campione precedente è *bin\Debug\netcoreapp1.1\publish*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-155">The actual path in the sample above is *bin\Debug\netcoreapp1.1\publish*.</span></span>

<span data-ttu-id="fbf3d-156">`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="fbf3d-157">Il comando seguente specifica un build `Release` e la directory di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="fbf3d-158">Il comando `dotnet publish` chiama `MSBuild` che richiama la destinazione `Publish`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-158">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="fbf3d-159">Tutti i parametri passati a `dotnet publish` vengono passati a `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-159">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="fbf3d-160">Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="fbf3d-161">Il parametro `-o` esegue il mapping a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="fbf3d-162">È possibile passare le proprietà di MSBuild usando uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-162">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="fbf3d-163">Il comando seguente pubblica un build `Release` a una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="fbf3d-164">La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="fbf3d-165">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="fbf3d-166">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="fbf3d-167">La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="fbf3d-168">Profili di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="fbf3d-168">Publish profiles</span></span>

<span data-ttu-id="fbf3d-169">Questa sezione usa Visual Studio 2017 e versioni successive per creare profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-169">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="fbf3d-170">Una volta creati, è possibile pubblicare da Visual Studio o dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-170">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="fbf3d-171">I profili di pubblicazione possono semplificare il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-171">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="fbf3d-172">È possibile avere più profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-172">You can have multiple publish profiles.</span></span> <span data-ttu-id="fbf3d-173">Per creare un profilo di pubblicazione in Visual Studio, fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-173">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="fbf3d-174">In alternativa, è possibile selezionare **Pubblica... \<nome progetto >** dal menu di compilazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-174">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="fbf3d-175">Viene visualizzata la scheda **Pubblica** della pagina di capacità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-175">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="fbf3d-176">Se il progetto non contiene un profilo di pubblicazione, viene visualizzata la pagina seguente:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-176">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza la cartella Azure, IIS, FTB con Azure selezionato.](web-publishing-vs/_static/az.png)

<span data-ttu-id="fbf3d-179">Selezionando **Cartella**, il pulsante **Pubblica** crea un profilo di pubblicazione della cartella ed esegue la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-179">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza la cartella Azure, IIS, FTB](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="fbf3d-181">Dopo aver creato un profilo di pubblicazione, la scheda **Pubblica** cambia ed è possibile selezionare **Crea nuovo profilo** per creare un nuovo profilo.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-181">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza il FolderProfile creato nell'ultimo passaggio e il collegamento Crea nuovo profilo](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="fbf3d-183">La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-183">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="fbf3d-184">Servizio app di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fbf3d-184">Microsoft Azure App Service</span></span>
- <span data-ttu-id="fbf3d-185">IIS, FTP e così via (per qualunque server Web)</span><span class="sxs-lookup"><span data-stu-id="fbf3d-185">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="fbf3d-186">Cartella</span><span class="sxs-lookup"><span data-stu-id="fbf3d-186">Folder</span></span>
- <span data-ttu-id="fbf3d-187">Importa profilo (consente di importare un profilo).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-187">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="fbf3d-188">Macchine virtuali di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fbf3d-188">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="fbf3d-189">Per altre informazioni vedere [Quali sono le opzioni di pubblicazione più adatte?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="fbf3d-189">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="fbf3d-190">Quando si crea un profilo di pubblicazione con Visual Studio, viene creato un file MSBuild *Properties/PublishProfiles/\<publish name>.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-190">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="fbf3d-191">Questo file *.pubxml* è un file MSBuild e contiene le impostazioni di configurazione della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-191">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="fbf3d-192">È possibile modificare questo file per personalizzare il processo di compilazione e di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-192">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="fbf3d-193">Questo file viene letto dal processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-193">This file is read by the publishing process.</span></span> <span data-ttu-id="fbf3d-194">`<LastUsedBuildConfiguration>`è speciale perché è una proprietà globale e non deve essere presente in qualunque file importato nella compilazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-194">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="fbf3d-195">Per altre informazioni vedere [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: come impostare la proprietà di configurazione).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-195">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="fbf3d-196">Il file *.pubxml* non deve essere selezionato nel controllo del codice sorgente perché dipende dal file *.user*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-196">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="fbf3d-197">Il file *.user* non deve mai essere selezionato nel controllo del codice sorgente perché può contenere informazioni riservate ed è valido solo per un unico utente e computer.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-197">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="fbf3d-198">Le informazioni riservate (come la password di pubblicazione) sono crittografate a livello di ogni utente/computer e archiviate nel file *Properties/PublishProfiles/\<publish name>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-198">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="fbf3d-199">Poiché questo file può contenere informazioni riservate, **non** deve essere selezionato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-199">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="fbf3d-200">Per una panoramica su come pubblicare un'app Web in ASP.NET Core vedere [Publishing and Deployment](index.md) (Pubblicazione e distribuzione).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-200">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="fbf3d-201">[Publishing and Deployment](index.md).(Pubblicazione e distribuzione) è un progetto open source in https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-201">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="fbf3d-202">Attualmente `dotnet publish` non ha la possibilità di usare i profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-202">Currently `dotnet publish` doesn’t have the ability to use publish profiles.</span></span> <span data-ttu-id="fbf3d-203">Per i profili di pubblicazione, usare `dotnet build`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-203">To use publish profiles, use `dotnet build`.</span></span> <span data-ttu-id="fbf3d-204">`dotnet build` richiama MSBuild nel progetto.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-204">`dotnet build` invokes MSBuild on the project.</span></span> <span data-ttu-id="fbf3d-205">In alternativa, chiamare `msbuild` direttamente.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-205">Alternatively, call `msbuild` directly.</span></span>

<span data-ttu-id="fbf3d-206">Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-206">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="fbf3d-207">Ad esempio, durante la pubblicazione con un profilo denominato *FolderProfile* è possibile eseguire uno dei comandi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-207">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="fbf3d-208">Richiamando `dotnet build` viene chiamato `msbuild` per eseguire il processo di compilazione e di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-208">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="fbf3d-209">È sostanzialmente equivalente chiamare `dotnet build` o `msbuild` quando si passa in un profilo cartella.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-209">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="fbf3d-210">Chiamando MSBuild direttamente in Windows si ottiene la versione .NET Framework di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-210">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="fbf3d-211">MSDeploy è attualmente limitato ai computer Windows per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-211">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="fbf3d-212">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-212">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="fbf3d-213">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-213">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="fbf3d-214">Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-214">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="fbf3d-215">Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-215">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="fbf3d-216">Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-216">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="fbf3d-217">Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-217">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="fbf3d-218">La proprietà di configurazione `<LastUsedBuildConfiguration>` è speciale e non deve essere sottoposta a override in un file di MSBuild importato.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-218">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="fbf3d-219">È possibile eseguire l'override di questa proprietà dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-219">You can override this property from the command line.</span></span> <span data-ttu-id="fbf3d-220">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-220">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="fbf3d-221">Mediante MSBuild:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-221">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="fbf3d-222">Pubblicare in un endpoint di MSDeploy dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="fbf3d-222">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="fbf3d-223">Come menzionato in precedenza, è possibile pubblicare usando `dotnet publish` o il comando `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-223">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="fbf3d-224">`dotnet publish` viene eseguito nel contesto di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-224">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="fbf3d-225">`msbuild` richiede .NET framework e pertanto è limitato agli ambienti Windows.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-225">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="fbf3d-226">Il modo più semplice per pubblicare con MSDeploy è quello di creare prima un profilo di pubblicazione in Visual Studio 2017, quindi usare il profilo dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-226">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="fbf3d-227">Nell'esempio seguente, è stata creata un'app Web di ASP.NET Core (usando `dotnet new mvc`) ed è stato aggiunto un profilo di pubblicazione di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-227">In the following sample, I created an ASP.NET Core web app ( using `dotnet new mvc`) and added an Azure publish profile with Visual Studio.</span></span>

<span data-ttu-id="fbf3d-228">Eseguire `msbuild` da un **Prompt dei comandi per gli sviluppatori per VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-228">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="fbf3d-229">Il prompt dei comandi per sviluppatori avrà il file *msbuild.exe* corrette nel proprio percorso e imposterà alcune variabili di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-229">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="fbf3d-230">MSBuild usa la sintassi seguente:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-230">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="fbf3d-231">È possibile ottenere la `Password` dal file *\<Publish name>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-231">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="fbf3d-232">È possibile scaricare il file *.PublishSettings* da:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-232">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="fbf3d-233">Esplora soluzioni: fare clic con il pulsante destro del mouse sull'app Web e selezionare **Scarica profilo di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-233">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="fbf3d-234">Portale di gestione di Azure: selezionare **Recupera profilo di pubblicazione** nel pannello App Web.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-234">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="fbf3d-235">Lo `Username` può essere trovato nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-235">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="fbf3d-236">Nel campione seguente viene usato il profilo di pubblicazione "Web11112 - Distribuzione Web":</span><span class="sxs-lookup"><span data-stu-id="fbf3d-236">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="fbf3d-237">Esclusione di file</span><span class="sxs-lookup"><span data-stu-id="fbf3d-237">Excluding files</span></span>

<span data-ttu-id="fbf3d-238">Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-238">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="fbf3d-239">`msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-239">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="fbf3d-240">Ad esempio, il markup dell'elemento `<Content>` seguente escluderà tutti i file di testo (*.txt*) dalla cartella *wwwroot/content* e da tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-240">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="fbf3d-241">Il markup riportato sopra può essere aggiunto a un profilo di pubblicazione o al file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-241">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="fbf3d-242">Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-242">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="fbf3d-243">Il markup dell'elemento `<MsDeploySkipRules>` seguente esclude tutti i file dalla cartella *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-243">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="fbf3d-244">`<MsDeploySkipRules>` non eliminerà le destinazioni da *ignorare* dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-244">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="fbf3d-245">Verranno eliminati dal sito di distribuzione i file e le cartelle di destinazione `<Content>`.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-245">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="fbf3d-246">Si supponga, ad esempio, di aver distribuito un'app Web con i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-246">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="fbf3d-247">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fbf3d-247">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="fbf3d-248">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fbf3d-248">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="fbf3d-249">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fbf3d-249">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="fbf3d-250">Se è stato aggiunto il markup `<MsDeploySkipRules>` seguente, questi file non verranno eliminati nel sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-250">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="fbf3d-251">Il markup `<MsDeploySkipRules>` visualizzato sopra impedisce che i file *ignorati* vengano distribuiti, ma non elimina tali file una volta distribuiti.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-251">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="fbf3d-252">Il markup `<Content>` seguente eliminerebbe i file di destinazione sul sito di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-252">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="fbf3d-253">L'uso della distribuzione della riga di comando con il markup `<Content>` riportato sopra produrrebbe un risultato simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-253">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="fbf3d-254">Inclusione di file</span><span class="sxs-lookup"><span data-stu-id="fbf3d-254">Including files</span></span>

<span data-ttu-id="fbf3d-255">Il markup seguente può essere usato per includere una cartella *immagini* all'esterno della directory di progetto nella cartella *wwwroot/images* del sito di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-255">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="fbf3d-256">Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-256">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="fbf3d-257">Se viene aggiunto al file *.csproj*, verrà incluso in ogni profilo di pubblicazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-257">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="fbf3d-258">Il markup evidenziato seguente illustra come:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-258">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="fbf3d-259">Copiare un file dall'esterno del progetto nella cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-259">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="fbf3d-260">Escludere la cartella *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-260">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="fbf3d-261">Escludere *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-261">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="fbf3d-262">Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-262">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="fbf3d-263">Eseguire una destinazione prima o dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="fbf3d-263">Run a target before or after publishing</span></span>

<span data-ttu-id="fbf3d-264">Le destinazioni `BeforePublish` e `AfterPublish` incorporate possono essere usate per eseguire una destinazione prima o dopo la destinazione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-264">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="fbf3d-265">È possibile aggiungere il markup seguente al profilo di pubblicazione per registrare messaggi nell'output di console prima e dopo la pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-265">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="fbf3d-266">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="fbf3d-266">The Kudu service</span></span>

<span data-ttu-id="fbf3d-267">Per visualizzare i file nella propria applicazione Web di Azure, usare il [servizio kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="fbf3d-267">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="fbf3d-268">Accodare il token `scm` al nome della propria app Web.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-268">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="fbf3d-269">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fbf3d-269">For example:</span></span>

| <span data-ttu-id="fbf3d-270">URL</span><span class="sxs-lookup"><span data-stu-id="fbf3d-270">URL</span></span>               | <span data-ttu-id="fbf3d-271">Risultato</span><span class="sxs-lookup"><span data-stu-id="fbf3d-271">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="fbf3d-272">App Web</span><span class="sxs-lookup"><span data-stu-id="fbf3d-272">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="fbf3d-273">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="fbf3d-273">Kudu sevice</span></span> |

<span data-ttu-id="fbf3d-274">Selezionare la voce di menu [Console di debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) per visualizzare/modificare/eliminare/aggiungere file.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-274">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbf3d-275">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fbf3d-275">Additional resources</span></span>

- <span data-ttu-id="fbf3d-276">[Distribuzione Web](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) semplifica la distribuzione di applicazioni Web e siti Web su server IIS.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-276">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="fbf3d-277">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemi di file e funzionalità di richiesta per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="fbf3d-277">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
