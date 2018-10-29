---
title: Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core
author: rick-anderson
description: Informazioni su come creare profili di pubblicazione in Visual Studio e usarli per la gestione delle distribuzioni di app ASP.NET Core in varie destinazioni.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3e626f99b06b0343360d6c46447e357890433dda
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148928"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="1db32-103">Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1db32-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="1db32-104">Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1db32-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1db32-105">Questo documento descrive come usare Visual Studio 2017 per la creazione e l'uso di profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="1db32-106">I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1db32-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="1db32-107">Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="1db32-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="1db32-108">Il file di progetto seguente è stato creato con il comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="1db32-108">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.7" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.8" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="1db32-109">L'attributo `Sdk` dell'elemento `<Project>` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="1db32-109">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="1db32-110">Importa il file delle proprietà da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.</span><span class="sxs-lookup"><span data-stu-id="1db32-110">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="1db32-111">Importa il file di destinazioni da  *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.</span><span class="sxs-lookup"><span data-stu-id="1db32-111">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="1db32-112">Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="1db32-112">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="1db32-113">L'SDK `Microsoft.NET.Sdk.Web` dipende da:</span><span class="sxs-lookup"><span data-stu-id="1db32-113">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="1db32-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="1db32-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="1db32-115">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="1db32-115">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="1db32-116">Che determina l'importazione delle proprietà e destinazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1db32-116">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="1db32-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="1db32-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="1db32-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="1db32-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="1db32-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="1db32-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="1db32-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="1db32-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="1db32-121">Le destinazioni di pubblicazione importano il set corretto di destinazioni in base al metodo di pubblicazione usato.</span><span class="sxs-lookup"><span data-stu-id="1db32-121">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="1db32-122">Quando MSBuild o Visual Studio carica un progetto, vengono eseguite le azioni di alto livello seguenti:</span><span class="sxs-lookup"><span data-stu-id="1db32-122">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="1db32-123">Compilare un progetto</span><span class="sxs-lookup"><span data-stu-id="1db32-123">Build project</span></span>
* <span data-ttu-id="1db32-124">Calcolare i file da pubblicare</span><span class="sxs-lookup"><span data-stu-id="1db32-124">Compute files to publish</span></span>
* <span data-ttu-id="1db32-125">Pubblicare i file nella destinazione</span><span class="sxs-lookup"><span data-stu-id="1db32-125">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="1db32-126">Calcolare gli elementi del progetto</span><span class="sxs-lookup"><span data-stu-id="1db32-126">Compute project items</span></span>

<span data-ttu-id="1db32-127">Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file).</span><span class="sxs-lookup"><span data-stu-id="1db32-127">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="1db32-128">L'attributo `item type` determina la modalità di elaborazione del file.</span><span class="sxs-lookup"><span data-stu-id="1db32-128">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="1db32-129">Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`.</span><span class="sxs-lookup"><span data-stu-id="1db32-129">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="1db32-130">I file presenti nell'elenco di elementi `Compile` vengono compilati.</span><span class="sxs-lookup"><span data-stu-id="1db32-130">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="1db32-131">L'elenco di elementi `Content` contiene i file che sono pubblicati in aggiunta agli output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-131">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="1db32-132">Per impostazione predefinita, i file corrispondenti al criterio `wwwroot/**` saranno inclusi nell'elemento `Content`.</span><span class="sxs-lookup"><span data-stu-id="1db32-132">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="1db32-133">Il [criterio GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot/\*\*` specifica tutti i file nella cartella *wwwroot* **e** relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="1db32-133">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="1db32-134">Per aggiungere esplicitamente un file all'elenco di pubblicazione, aggiungere il file direttamente nel file *.csproj*, come illustrato in [File di inclusione](#include-files).</span><span class="sxs-lookup"><span data-stu-id="1db32-134">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="1db32-135">Quando si seleziona il pulsante **Pubblica** in Visual Studio o quando si pubblica dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="1db32-135">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="1db32-136">Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).</span><span class="sxs-lookup"><span data-stu-id="1db32-136">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="1db32-137">**Solo Visual Studio**: i pacchetti NuGet vengono ripristinati.</span><span class="sxs-lookup"><span data-stu-id="1db32-137">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="1db32-138">(Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)</span><span class="sxs-lookup"><span data-stu-id="1db32-138">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="1db32-139">Il progetto viene compilato.</span><span class="sxs-lookup"><span data-stu-id="1db32-139">The project builds.</span></span>
* <span data-ttu-id="1db32-140">Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).</span><span class="sxs-lookup"><span data-stu-id="1db32-140">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="1db32-141">Il progetto viene pubblicato (i file calcolati vengono copiati nella destinazione di pubblicazione).</span><span class="sxs-lookup"><span data-stu-id="1db32-141">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="1db32-142">Quando un progetto ASP.NET Core fa riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto, nella radice della directory dell'app Web viene posizionato un file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1db32-142">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="1db32-143">Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1db32-143">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="1db32-144">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="1db32-144">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="1db32-145">Pubblicazione di base dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="1db32-145">Basic command-line publishing</span></span>

<span data-ttu-id="1db32-146">La pubblicazione dalla riga di comando funziona su tutte le piattaforme supportate da .NET Core e non richiede Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1db32-146">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="1db32-147">Negli esempi seguenti, il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) viene eseguito dalla directory del progetto (che contiene il file *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="1db32-147">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="1db32-148">Se non si è nella cartella del progetto, passare esplicitamente il percorso del file del progetto.</span><span class="sxs-lookup"><span data-stu-id="1db32-148">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="1db32-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1db32-149">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="1db32-150">Eseguire i comandi seguenti per creare e pubblicare un'app Web:</span><span class="sxs-lookup"><span data-stu-id="1db32-150">Run the following commands to create and publish a web app:</span></span>

::: moniker range=">= aspnetcore-2.0"

```console
dotnet new mvc
dotnet publish
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```console
dotnet new mvc
dotnet restore
dotnet publish
```

::: moniker-end

<span data-ttu-id="1db32-151">Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) genera un output analogo a quello illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1db32-151">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="1db32-152">La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="1db32-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="1db32-153">Il valore predefinito per `$(Configuration)` è *Debug*.</span><span class="sxs-lookup"><span data-stu-id="1db32-153">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="1db32-154">Nell'esempio precedente, `<TargetFramework>` è `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="1db32-154">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="1db32-155">`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-155">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="1db32-156">Il comando seguente specifica un build `Release` e la directory di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="1db32-156">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="1db32-157">Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) chiama MSBuild, che richiama la destinazione `Publish`.</span><span class="sxs-lookup"><span data-stu-id="1db32-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="1db32-158">Tutti i parametri passati a `dotnet publish` vengono passati a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1db32-158">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="1db32-159">Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="1db32-159">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="1db32-160">Il parametro `-o` esegue il mapping a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="1db32-160">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="1db32-161">È possibile passare le proprietà di MSBuild usando uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="1db32-161">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="1db32-162">Il comando seguente pubblica un build `Release` a una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="1db32-162">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="1db32-163">La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="1db32-163">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="1db32-164">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1db32-164">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="1db32-165">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1db32-165">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="1db32-166">La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.</span><span class="sxs-lookup"><span data-stu-id="1db32-166">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="1db32-167">Profili di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="1db32-167">Publish profiles</span></span>

<span data-ttu-id="1db32-168">Questa sezione usa Visual Studio 2017 per creare un profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-168">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="1db32-169">Una volta creato, è disponibile la pubblicazione da Visual Studio o dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1db32-169">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="1db32-170">I profili di pubblicazione possono semplificare il processo di pubblicazione e può essere presente un numero illimitato di profili.</span><span class="sxs-lookup"><span data-stu-id="1db32-170">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="1db32-171">Creare un profilo di pubblicazione in Visual Studio scegliendo uno dei seguenti percorsi:</span><span class="sxs-lookup"><span data-stu-id="1db32-171">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="1db32-172">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="1db32-172">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="1db32-173">Selezionare **Pubblica &lt;nome_progetto&gt;** dal menu **Compila**.</span><span class="sxs-lookup"><span data-stu-id="1db32-173">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="1db32-174">Verrà visualizzata la scheda **Pubblica** della pagina di capacità dell'app.</span><span class="sxs-lookup"><span data-stu-id="1db32-174">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="1db32-175">Se il progetto non dispone di un profilo di pubblicazione, viene visualizzata la pagina seguente:</span><span class="sxs-lookup"><span data-stu-id="1db32-175">If the project lacks a publish profile, the following page is displayed:</span></span>

![Scheda Pubblica della pagina di capacità dell'app](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="1db32-177">Quando è selezionata l'opzione **Cartella**, specificare il percorso di una cartella in cui archiviare gli asset pubblicati.</span><span class="sxs-lookup"><span data-stu-id="1db32-177">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="1db32-178">La cartella predefinita è *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="1db32-178">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="1db32-179">Fare clic sul pulsante **Crea profilo** per terminare.</span><span class="sxs-lookup"><span data-stu-id="1db32-179">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="1db32-180">Dopo aver creato un profilo di pubblicazione, la scheda **Pubblica** cambia.</span><span class="sxs-lookup"><span data-stu-id="1db32-180">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="1db32-181">Il nuovo profilo creato viene visualizzato in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="1db32-181">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="1db32-182">Fare clic su **Crea nuovo profilo** per creare un altro nuovo profilo.</span><span class="sxs-lookup"><span data-stu-id="1db32-182">Click **Create new profile** to create another new profile.</span></span>

![Scheda Pubblica della pagina di capacità dell'app che visualizza FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="1db32-184">La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="1db32-184">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="1db32-185">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="1db32-185">Azure App Service</span></span>
* <span data-ttu-id="1db32-186">Macchine virtuali di Azure</span><span class="sxs-lookup"><span data-stu-id="1db32-186">Azure Virtual Machines</span></span>
* <span data-ttu-id="1db32-187">IIS, FTP e così via (per qualunque server Web)</span><span class="sxs-lookup"><span data-stu-id="1db32-187">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="1db32-188">Cartella</span><span class="sxs-lookup"><span data-stu-id="1db32-188">Folder</span></span>
* <span data-ttu-id="1db32-189">Importa profilo</span><span class="sxs-lookup"><span data-stu-id="1db32-189">Import Profile</span></span>

<span data-ttu-id="1db32-190">Per altre informazioni, vedere [Quali sono le opzioni di pubblicazione più adatte?](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="1db32-190">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="1db32-191">Quando si crea un profilo di pubblicazione con Visual Studio, viene creato un file di MSBuild *Properties/PublishProfiles/&lt;nome_profilo&gt;.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="1db32-191">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="1db32-192">Il file *.pubxml* è un file di MSBuild e contiene le impostazioni di configurazione della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-192">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="1db32-193">Questo file può essere modificato per personalizzare il processo di compilazione e di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-193">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="1db32-194">Questo file viene letto dal processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-194">This file is read by the publishing process.</span></span> <span data-ttu-id="1db32-195">`<LastUsedBuildConfiguration>` è speciale perché è una proprietà globale e non deve essere presente in alcun file importato nella compilazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-195">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="1db32-196">Per altre informazioni, vedere [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: come impostare la proprietà di configurazione).</span><span class="sxs-lookup"><span data-stu-id="1db32-196">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="1db32-197">Quando si esegue la pubblicazione in una destinazione di Azure, il file *.pubxml* contiene l'identificatore della sottoscrizione Azure.</span><span class="sxs-lookup"><span data-stu-id="1db32-197">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="1db32-198">Con tale tipo di destinazione, è sconsigliato aggiungere questo file al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1db32-198">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="1db32-199">Quando si pubblica in una destinazione non Azure, è possibile archiviare il file *.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="1db32-199">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="1db32-200">Le informazioni riservate, come la password di pubblicazione, vengono crittografate a livello di singolo utente/computer.</span><span class="sxs-lookup"><span data-stu-id="1db32-200">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="1db32-201">Sono memorizzate nel file *Properties/PublishProfiles/&lt;nome_profilo&gt;.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="1db32-201">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="1db32-202">Poiché questo file può contenere informazioni riservate, non deve essere archiviato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1db32-202">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="1db32-203">Per una panoramica su come pubblicare un'app Web in ASP.NET Core, vedere [Ospitare e distribuire](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="1db32-203">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="1db32-204">Le attività e le destinazioni di MSBuild necessarie per pubblicare un'app ASP.NET Core sono open source in https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="1db32-204">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="1db32-205">`dotnet publish` può usare profili di pubblicazione di tipo cartella, MSDeploy e [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="1db32-205">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="1db32-206">Cartella (multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="1db32-206">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="1db32-207">MSDeploy (attualmente funziona solo in Windows poiché MSDeploy non è multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="1db32-207">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="1db32-208">Pacchetto MSDeploy (attualmente funziona solo in Windows poiché MSDeploy non è multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="1db32-208">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="1db32-209">Negli esempi precedenti **non** passare `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="1db32-209">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="1db32-210">Per altre informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="1db32-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="1db32-211">`dotnet publish` supporta le API Kudu per la pubblicazione in Azure da qualsiasi piattaforma.</span><span class="sxs-lookup"><span data-stu-id="1db32-211">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="1db32-212">La pubblicazione con Visual Studio supporta le API Kudu, ma è supportata da WebSDK per la pubblicazione multipiattaforma in Azure.</span><span class="sxs-lookup"><span data-stu-id="1db32-212">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="1db32-213">Aggiungere un profilo di pubblicazione alla cartella *Properties/PublishProfiles* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="1db32-213">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="1db32-214">Eseguire il comando seguente per comprimere il contenuto per la pubblicazione e pubblicarlo in Azure tramite le API Kudu:</span><span class="sxs-lookup"><span data-stu-id="1db32-214">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="1db32-215">Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:</span><span class="sxs-lookup"><span data-stu-id="1db32-215">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="1db32-216">Durante la pubblicazione con un profilo denominato *FolderProfile*, è possibile eseguire uno dei comandi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="1db32-216">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="1db32-217">Richiamando [dotnet build](/dotnet/core/tools/dotnet-build), viene chiamato `msbuild` per eseguire il processo di compilazione e di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-217">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="1db32-218">Quando si passa un profilo di cartella, è equivalente chiamare `dotnet build` o `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="1db32-218">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="1db32-219">Chiamando MSBuild direttamente in Windows, viene usata la versione .NET Framework di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1db32-219">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="1db32-220">MSDeploy è attualmente limitato ai computer Windows per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="1db32-221">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle.</span><span class="sxs-lookup"><span data-stu-id="1db32-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="1db32-222">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows).</span><span class="sxs-lookup"><span data-stu-id="1db32-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="1db32-223">Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.</span><span class="sxs-lookup"><span data-stu-id="1db32-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="1db32-224">Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="1db32-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="1db32-225">Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`.</span><span class="sxs-lookup"><span data-stu-id="1db32-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="1db32-226">Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="1db32-227">La proprietà di configurazione `<LastUsedBuildConfiguration>` è speciale e non deve essere sottoposta a override in un file di MSBuild importato.</span><span class="sxs-lookup"><span data-stu-id="1db32-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="1db32-228">È possibile eseguire l'override di questa proprietà dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1db32-228">This property can be overridden from the command line.</span></span>

<span data-ttu-id="1db32-229">Mediante l'interfaccia della riga di comando di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="1db32-229">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="1db32-230">Mediante MSBuild:</span><span class="sxs-lookup"><span data-stu-id="1db32-230">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="1db32-231">Pubblicare in un endpoint di MSDeploy dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="1db32-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="1db32-232">La pubblicazione può essere eseguita tramite l'interfaccia della riga di comando di .NET Core o MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1db32-232">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="1db32-233">`dotnet publish` viene eseguito nel contesto di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1db32-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="1db32-234">Il comando `msbuild` richiede .NET Framework, che ne limita l'uso agli ambienti Windows.</span><span class="sxs-lookup"><span data-stu-id="1db32-234">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="1db32-235">Il modo più semplice per pubblicare con MSDeploy è quello di creare prima un profilo di pubblicazione in Visual Studio 2017, quindi usare il profilo dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1db32-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="1db32-236">Nell'esempio seguente è stata creata un'app Web ASP.NET Core usando `dotnet new mvc` ed è stato aggiunto un profilo di pubblicazione di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1db32-236">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="1db32-237">Eseguire `msbuild` da un **Prompt dei comandi per gli sviluppatori per VS2017**.</span><span class="sxs-lookup"><span data-stu-id="1db32-237">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="1db32-238">Il prompt dei comandi per gli sviluppatori ha il file *msbuild.exe* corretto nel proprio percorso, con alcune variabili di MSBuild impostate.</span><span class="sxs-lookup"><span data-stu-id="1db32-238">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="1db32-239">MSBuild usa la sintassi seguente:</span><span class="sxs-lookup"><span data-stu-id="1db32-239">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="1db32-240">Ottenere la `Password` dal file *\<Nome pubblicazione>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="1db32-240">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="1db32-241">Scaricare il file *.PublishSettings* da:</span><span class="sxs-lookup"><span data-stu-id="1db32-241">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="1db32-242">Esplora soluzioni: fare clic con il pulsante destro del mouse sull'app Web e selezionare **Scarica profilo di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="1db32-242">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="1db32-243">Portale di Azure: fare clic su **Recupera profilo di pubblicazione** nel pannello **Panoramica** dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="1db32-243">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="1db32-244">Lo `Username` può essere trovato nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="1db32-245">Nell'esempio riportato di seguito viene usato il profilo di pubblicazione *Web11112 - Distribuzione Web*:</span><span class="sxs-lookup"><span data-stu-id="1db32-245">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="1db32-246">Escludere file</span><span class="sxs-lookup"><span data-stu-id="1db32-246">Exclude files</span></span>

<span data-ttu-id="1db32-247">Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1db32-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="1db32-248">`msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="1db32-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="1db32-249">Ad esempio, l'elemento `<Content>` seguente esclude tutti i file di testo (*.txt*) dalla cartella *wwwroot/content* e da tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="1db32-249">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1db32-250">Il markup precedente può essere aggiunto a un profilo di pubblicazione o al file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1db32-250">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="1db32-251">Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="1db32-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="1db32-252">L'elemento `<MsDeploySkipRules>` seguente esclude tutti i file dalla cartella *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="1db32-252">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="1db32-253">`<MsDeploySkipRules>` non elimina le destinazioni da *ignorare* dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1db32-253">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="1db32-254">Vengono eliminati dal sito di distribuzione i file e le cartelle di destinazione `<Content>`.</span><span class="sxs-lookup"><span data-stu-id="1db32-254">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="1db32-255">Si supponga, ad esempio, che un'app Web distribuita contenga i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="1db32-255">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="1db32-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1db32-256">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="1db32-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1db32-257">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="1db32-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1db32-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="1db32-259">Se si aggiungono gli elementi `<MsDeploySkipRules>` seguenti, questi file non vengono eliminati nel sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1db32-259">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
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

<span data-ttu-id="1db32-260">Gli elementi `<MsDeploySkipRules>` precedenti impediscono che i file *ignorati* vengano distribuiti.</span><span class="sxs-lookup"><span data-stu-id="1db32-260">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="1db32-261">Tali file non verranno eliminati una volta distribuiti.</span><span class="sxs-lookup"><span data-stu-id="1db32-261">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="1db32-262">L'elemento `<Content>` seguente elimina i file di destinazione sul sito di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="1db32-262">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1db32-263">L'uso della distribuzione dalla riga di comando con l'elemento `<Content>` precedente produce il seguente output:</span><span class="sxs-lookup"><span data-stu-id="1db32-263">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
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

## <a name="include-files"></a><span data-ttu-id="1db32-264">File di inclusione</span><span class="sxs-lookup"><span data-stu-id="1db32-264">Include files</span></span>

<span data-ttu-id="1db32-265">Il markup seguente include una cartella *images* all'esterno della directory di progetto nella cartella *wwwroot/images* del sito di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="1db32-265">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="1db32-266">Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-266">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="1db32-267">Se viene aggiunto al file *.csproj*, è incluso in ogni profilo di pubblicazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="1db32-267">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="1db32-268">Il markup evidenziato seguente illustra come:</span><span class="sxs-lookup"><span data-stu-id="1db32-268">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="1db32-269">Copiare un file dall'esterno del progetto nella cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1db32-269">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="1db32-270">Escludere la cartella *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="1db32-270">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="1db32-271">Escludere *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1db32-271">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="1db32-272">Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="1db32-272">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="1db32-273">Eseguire una destinazione prima o dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="1db32-273">Run a target before or after publishing</span></span>

<span data-ttu-id="1db32-274">Le destinazioni incorporate `BeforePublish` e `AfterPublish` consentono di eseguire una destinazione prima o dopo la destinazione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1db32-274">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="1db32-275">Aggiungere gli elementi seguenti al profilo di pubblicazione per registrare i messaggi della console prima e dopo la pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="1db32-275">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="1db32-276">Pubblicazione in un server tramite un certificato non attendibile</span><span class="sxs-lookup"><span data-stu-id="1db32-276">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="1db32-277">Aggiungere la proprietà `<AllowUntrustedCertificate>` con un valore `True` al profilo di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="1db32-277">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="1db32-278">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="1db32-278">The Kudu service</span></span>

<span data-ttu-id="1db32-279">Per visualizzare i file in una distribuzione di app Web di Servizio app di Azure, usare il [servizio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="1db32-279">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="1db32-280">Accodare il token `scm` al nome dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="1db32-280">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="1db32-281">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1db32-281">For example:</span></span>

| <span data-ttu-id="1db32-282">URL</span><span class="sxs-lookup"><span data-stu-id="1db32-282">URL</span></span>                                    | <span data-ttu-id="1db32-283">Risultato</span><span class="sxs-lookup"><span data-stu-id="1db32-283">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="1db32-284">App Web</span><span class="sxs-lookup"><span data-stu-id="1db32-284">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="1db32-285">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="1db32-285">Kudu service</span></span> |

<span data-ttu-id="1db32-286">Selezionare la voce di menu [Console di debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) per visualizzare, modificare, eliminare o aggiungere file.</span><span class="sxs-lookup"><span data-stu-id="1db32-286">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1db32-287">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1db32-287">Additional resources</span></span>

* <span data-ttu-id="1db32-288">[Distribuzione Web](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) semplifica la distribuzione di app Web e siti Web sui server IIS.</span><span class="sxs-lookup"><span data-stu-id="1db32-288">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="1db32-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemi di file e funzionalità di richiesta per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1db32-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="1db32-290">Pubblicare un'app Web ASP.NET in una macchina virtuale di Azure da Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1db32-290">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
