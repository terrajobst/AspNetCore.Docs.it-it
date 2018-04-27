---
title: Visual Studio profili di pubblicazione per la distribuzione di app ASP.NET Core
author: rick-anderson
description: Informazioni su come creare profili di pubblicazione in Visual Studio e utilizzarli per la gestione delle distribuzioni di app ASP.NET Core per destinazioni diverse.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="6bbc0-103">Visual Studio profili di pubblicazione per la distribuzione di app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bbc0-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="6bbc0-104">Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6bbc0-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6bbc0-105">Questo documento si concentra sull'uso di Visual Studio 2017 per creare e usare profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="6bbc0-106">I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="6bbc0-107">Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="6bbc0-108">Il seguente file di progetto è stato creato con il comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6bbc0-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6bbc0-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6bbc0-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6bbc0-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="6bbc0-111">Il `<Project>` dell'elemento `Sdk` attributo eseguite le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="6bbc0-112">Importa il file delle proprietà da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="6bbc0-113">Importa il file di destinazioni da  *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="6bbc0-114">Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="6bbc0-115">Il `Microsoft.NET.Sdk.Web` dipende SDK:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="6bbc0-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="6bbc0-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="6bbc0-118">Che genera le seguenti proprietà e le destinazioni da importare:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="6bbc0-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="6bbc0-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="6bbc0-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="6bbc0-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="6bbc0-123">Pubblicare l'importazione delle destinazioni destra set di destinazioni in base al metodo di pubblicazione utilizzato.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="6bbc0-124">Quando viene caricato un progetto MSBuild o Visual Studio, si verificano le azioni di alto livello seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="6bbc0-125">Compilare un progetto</span><span class="sxs-lookup"><span data-stu-id="6bbc0-125">Build project</span></span>
* <span data-ttu-id="6bbc0-126">Calcolare i file da pubblicare</span><span class="sxs-lookup"><span data-stu-id="6bbc0-126">Compute files to publish</span></span>
* <span data-ttu-id="6bbc0-127">Pubblicare i file nella destinazione</span><span class="sxs-lookup"><span data-stu-id="6bbc0-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="6bbc0-128">Calcolare gli elementi del progetto</span><span class="sxs-lookup"><span data-stu-id="6bbc0-128">Compute project items</span></span>

<span data-ttu-id="6bbc0-129">Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="6bbc0-130">L'attributo `item type` determina la modalità di elaborazione del file.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="6bbc0-131">Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="6bbc0-132">I file presenti nell'elenco di elementi `Compile` vengono compilati.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="6bbc0-133">Il `Content` elenco di elementi contiene i file che vengono pubblicati oltre gli output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="6bbc0-134">Per impostazione predefinita, i file corrispondenti al criterio `wwwroot/**` sono inclusi nel `Content` elemento.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="6bbc0-135">Il `wwwroot/\*\*` [motivo il glob](https://gruntjs.com/configuring-tasks#globbing-patterns) corrisponde a tutti i file nel *wwwroot* cartella **e** le sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="6bbc0-136">Per aggiungere in modo esplicito un file all'elenco di pubblicazione, aggiungere il file direttamente nella *csproj* del file come illustrato nel [i file di inclusione](#include-files).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="6bbc0-137">Quando si seleziona il **pubblica** button in Visual Studio o quando si pubblicano dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="6bbc0-138">Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="6bbc0-139">**Visual Studio solo**: vengono ripristinati i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="6bbc0-140">(Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)</span><span class="sxs-lookup"><span data-stu-id="6bbc0-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="6bbc0-141">Il progetto viene compilato.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-141">The project builds.</span></span>
* <span data-ttu-id="6bbc0-142">Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="6bbc0-143">Il progetto viene pubblicato (calcolate vengono copiati nella destinazione di pubblicazione).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="6bbc0-144">Quando fa riferimento a un progetto ASP.NET Core `Microsoft.NET.Sdk.Web` nel file di progetto, un *app_offline.htm* file si trova nella radice della directory dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="6bbc0-145">Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="6bbc0-146">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="6bbc0-147">Pubblicazione della riga di comando di base</span><span class="sxs-lookup"><span data-stu-id="6bbc0-147">Basic command-line publishing</span></span>

<span data-ttu-id="6bbc0-148">La pubblicazione della riga di comando funziona su tutte le piattaforme supportate da .NET Core e non richiede Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="6bbc0-149">Negli esempi seguenti, il [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) comando viene eseguito dalla directory del progetto (che contiene il *csproj* file).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="6bbc0-150">Se non è nella cartella del progetto, passare in modo esplicito il percorso del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="6bbc0-151">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="6bbc0-152">Eseguire i comandi seguenti per creare e pubblicare un'app Web:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6bbc0-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6bbc0-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6bbc0-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6bbc0-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="6bbc0-155">Il [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) comando restituisce un output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="6bbc0-156">La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="6bbc0-157">Il valore predefinito per `$(Configuration)` viene *Debug*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="6bbc0-158">Nell'esempio precedente, il `<TargetFramework>` è `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="6bbc0-159">`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="6bbc0-160">Il comando seguente specifica un build `Release` e la directory di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="6bbc0-161">Il [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) comando MSBuild, che consente di visualizzare le chiamate di `Publish` destinazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="6bbc0-162">I parametri passati a `dotnet publish` vengono passati a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="6bbc0-163">Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="6bbc0-164">Il parametro `-o` esegue il mapping a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="6bbc0-165">Proprietà di MSBuild può essere passata tramite uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="6bbc0-166">Il comando seguente pubblica un build `Release` a una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="6bbc0-167">La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="6bbc0-168">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="6bbc0-169">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="6bbc0-170">La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="6bbc0-171">Profili di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="6bbc0-171">Publish profiles</span></span>

<span data-ttu-id="6bbc0-172">In questa sezione Usa 2017 di Visual Studio per creare un profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="6bbc0-173">Una volta creata, la pubblicazione da Visual Studio o la riga di comando è disponibile.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="6bbc0-174">Pubblicare i profili possono semplificare il processo di pubblicazione e può contenere qualsiasi numero di profili.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="6bbc0-175">Creare un profilo di pubblicazione in Visual Studio scegliendo uno dei seguenti percorsi:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="6bbc0-176">Fare clic sul progetto in Esplora soluzioni e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="6bbc0-177">Selezionare **pubblica &lt;project_name&gt;**  dal **compilare** menu.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="6bbc0-178">Il **pubblica** viene visualizzata la scheda della pagina capacità app.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="6bbc0-179">Se il progetto non dispone di un profilo di pubblicazione, viene visualizzata la pagina seguente:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Scheda pubblica la pagina di capacità di app](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="6bbc0-181">Quando si **cartella** è selezionata, specificare un percorso di cartella per archiviare le risorse pubblicate.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="6bbc0-182">La cartella predefinita è *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="6bbc0-183">Fare clic sui **Crea profilo** pulsante Fine.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="6bbc0-184">Una volta creato un profilo di pubblicazione, il **pubblica** scheda modifiche.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="6bbc0-185">Il profilo appena creato viene visualizzato in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="6bbc0-186">Fare clic su **Crea nuovo profilo** per creare un altro nuovo profilo.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-186">Click **Create new profile** to create another new profile.</span></span>

![Scheda pubblica la pagina di capacità di app che mostra FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="6bbc0-188">La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="6bbc0-189">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="6bbc0-189">Azure App Service</span></span>
* <span data-ttu-id="6bbc0-190">Macchine virtuali di Azure</span><span class="sxs-lookup"><span data-stu-id="6bbc0-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="6bbc0-191">IIS, FTP e così via (per qualsiasi server web)</span><span class="sxs-lookup"><span data-stu-id="6bbc0-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="6bbc0-192">Cartella</span><span class="sxs-lookup"><span data-stu-id="6bbc0-192">Folder</span></span>
* <span data-ttu-id="6bbc0-193">Importare il profilo</span><span class="sxs-lookup"><span data-stu-id="6bbc0-193">Import Profile</span></span>

<span data-ttu-id="6bbc0-194">Per altre informazioni, vedere [quali opzioni di pubblicazione sono adatta alle mie esigenze](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="6bbc0-195">Quando si crea un profilo di pubblicazione con Visual Studio, un *proprietà/PublishProfiles/&lt;profile_name&gt;pubxml* viene creato il file MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="6bbc0-196">Il *pubxml* file è un file di MSBuild e contiene le impostazioni di configurazione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="6bbc0-197">Questo file può essere modificato per personalizzare la compilazione e processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="6bbc0-198">Questo file viene letto dal processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-198">This file is read by the publishing process.</span></span> <span data-ttu-id="6bbc0-199">`<LastUsedBuildConfiguration>` è uno speciale perché è una proprietà globale e non deve essere in qualsiasi file che viene importato nella compilazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="6bbc0-200">Vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="6bbc0-201">Quando si pubblica in una destinazione di Azure, il *pubxml* file contiene l'identificatore della sottoscrizione Azure.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="6bbc0-202">Con tale tipo di destinazione, è sconsigliato l'aggiunta di questo file al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="6bbc0-203">Quando si pubblica in una destinazione non Azure, è consigliabile archiviare il *pubxml* file.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="6bbc0-204">Informazioni riservate (ad esempio la password di pubblicazione) vengono crittografate in un livello di utente/computer.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="6bbc0-205">In cui è memorizzato il *proprietà/PublishProfiles/&lt;profile_name&gt;. pubxml.user* file.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="6bbc0-206">Poiché questo file è possibile archiviare le informazioni riservate, non deve essere verificato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="6bbc0-207">Per una panoramica di come pubblicare un'app web in ASP.NET Core, vedere [Host e distribuire](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="6bbc0-208">Le attività di MSBuild e destinazioni necessarie per pubblicare un'app di ASP.NET Core sono open source in https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="6bbc0-209">`dotnet publish` può utilizzare cartella, MSDeploy, e [Kudu](https://github.com/projectkudu/kudu/wiki) profili di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="6bbc0-210">Cartella (e multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="6bbc0-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="6bbc0-211">MSDeploy (attualmente questo funziona solo in Windows poiché MSDeploy non multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="6bbc0-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="6bbc0-212">Pacchetto di MSDeploy (attualmente questo funziona solo in Windows poiché MSDeploy non multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="6bbc0-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="6bbc0-213">Negli esempi precedenti **non** passare `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="6bbc0-214">Per ulteriori informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="6bbc0-215">`dotnet publish` supporta le API Kudu per pubblicare in Azure da qualsiasi piattaforma.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="6bbc0-216">Visual Studio la pubblicazione supporta le API Kudu, ma è supportato dal WebSDK per multipiattaforma pubblicare in Azure.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="6bbc0-217">Aggiungere un profilo di pubblicazione per il *proprietà/PublishProfiles* cartella con il seguente contenuto:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="6bbc0-218">Eseguire il comando seguente per comprimere il contenuto di pubblicazione e pubblicarla in Azure utilizzando le API Kudu:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="6bbc0-219">Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="6bbc0-220">Durante la pubblicazione con un profilo denominato *FolderProfile*, è possibile eseguire uno dei comandi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="6bbc0-221">Quando si richiama [compilazione dotnet](/dotnet/core/tools/dotnet-build), chiama `msbuild` per eseguire la compilazione e processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="6bbc0-222">Una chiamata a `dotnet build` o `msbuild` equivale quando vengono passati in un profilo di cartella.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="6bbc0-223">Quando si chiama MSBuild direttamente in Windows, viene utilizzata la versione di .NET Framework di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="6bbc0-224">MSDeploy è attualmente limitato ai computer Windows per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="6bbc0-225">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="6bbc0-226">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="6bbc0-227">Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="6bbc0-228">Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="6bbc0-229">Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="6bbc0-230">Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="6bbc0-231">Il `<LastUsedBuildConfiguration>` proprietà di configurazione è speciale e non deve essere sottoposto a override in un file di MSBuild importato.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="6bbc0-232">Questa proprietà può essere sottoposto a override dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="6bbc0-233">Utilizzo di .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="6bbc0-234">Mediante MSBuild:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="6bbc0-235">Pubblicare in un endpoint di MSDeploy dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="6bbc0-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="6bbc0-236">La pubblicazione può essere eseguita utilizzando l'interfaccia CLI di .NET Core o MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="6bbc0-237">`dotnet publish` viene eseguito nel contesto di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="6bbc0-238">Il `msbuild` comando richiede .NET Framework, che limita agli ambienti di Windows.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="6bbc0-239">Il modo più semplice per pubblicare con MSDeploy è quello di creare prima un profilo di pubblicazione in Visual Studio 2017, quindi usare il profilo dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="6bbc0-240">Nell'esempio seguente viene creata un'app web ASP.NET Core (utilizzando `dotnet new mvc`), e viene aggiunto un profilo di pubblicazione di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="6bbc0-241">Eseguire `msbuild` da un **prompt dei comandi per sviluppatori per Visual Studio 2017**.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="6bbc0-242">Prompt dei comandi per sviluppatori è corrette *msbuild.exe* nel percorso con un set di variabili MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="6bbc0-243">MSBuild usa la sintassi seguente:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="6bbc0-244">Ottenere il `Password` dal  *\<nome pubblicazione >. PublishSettings* file.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="6bbc0-245">Scaricare il *. PublishSettings* file da uno:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="6bbc0-246">Esplora soluzioni: Nell'App Web e scegliere **Scarica profilo di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="6bbc0-247">Portale di Azure: fare clic su **profilo di pubblicazione Get** dell'App Web **Panoramica** pannello.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="6bbc0-248">Lo `Username` può essere trovato nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="6bbc0-249">L'esempio seguente usa il *Web11112 - distribuzione Web* profilo di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="6bbc0-250">Escludere i file</span><span class="sxs-lookup"><span data-stu-id="6bbc0-250">Exclude files</span></span>

<span data-ttu-id="6bbc0-251">Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="6bbc0-252">`msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="6bbc0-253">Ad esempio, il seguente `<Content>` elemento esclude tutto il testo (*. txt*) i file dal *wwwroot/content* cartella e tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="6bbc0-254">Il markup precedente può essere aggiunti a un profilo di pubblicazione o il *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="6bbc0-255">Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="6bbc0-256">I seguenti `<MsDeploySkipRules>` elemento esclude tutti i file dal *wwwroot/content* cartella:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="6bbc0-257">`<MsDeploySkipRules>` non verrà eliminato il *ignorare* destinazioni dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="6bbc0-258">`<Content>` cartelle e file di destinazione vengono eliminate dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="6bbc0-259">Si supponga, ad esempio, che un'app web distribuite ha i seguenti file:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="6bbc0-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="6bbc0-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="6bbc0-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6bbc0-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="6bbc0-263">Se i seguenti `<MsDeploySkipRules>` gli elementi vengono aggiunti, non è possibile eliminare tali file nel sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="6bbc0-264">Precedenti `<MsDeploySkipRules>` elementi impediscono la *ignorata* file venga distribuito.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="6bbc0-265">Tali file non verrà eliminato una volta vengano distribuite.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="6bbc0-266">Nell'esempio `<Content>` elemento Elimina i file di destinazione nel sito di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="6bbc0-267">Utilizzando la distribuzione della riga di comando con il precedente `<Content>` elemento produce il seguente output:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="6bbc0-268">File di inclusione</span><span class="sxs-lookup"><span data-stu-id="6bbc0-268">Include files</span></span>

<span data-ttu-id="6bbc0-269">Il markup seguente include un *immagini* cartella all'esterno della directory di progetto per il *wwwroot/immagini* cartella del sito pubblica:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="6bbc0-270">Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="6bbc0-271">Se viene aggiunto il *csproj* file, è incluso in ogni profilo di pubblicazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="6bbc0-272">Il markup evidenziato seguente illustra come:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="6bbc0-273">Copiare un file dall'esterno del progetto nella cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="6bbc0-274">Escludere la cartella *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="6bbc0-275">Escludere *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="6bbc0-276">Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="6bbc0-277">Eseguire una destinazione prima o dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="6bbc0-277">Run a target before or after publishing</span></span>

<span data-ttu-id="6bbc0-278">L'elemento predefinito `BeforePublish` e `AfterPublish` destinazioni eseguire una destinazione prima o dopo la destinazione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="6bbc0-279">Aggiungere i seguenti elementi per il profilo di pubblicazione per registrare i messaggi della console prima e dopo la pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="6bbc0-280">La pubblicazione in un server utilizzando un certificato non attendibile</span><span class="sxs-lookup"><span data-stu-id="6bbc0-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="6bbc0-281">Aggiungere il `<AllowUntrustedCertificate>` proprietà con un valore di `True` per il profilo di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="6bbc0-282">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="6bbc0-282">The Kudu service</span></span>

<span data-ttu-id="6bbc0-283">Per visualizzare i file in una distribuzione di app web di servizio App di Azure, usare il [servizio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="6bbc0-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="6bbc0-284">Aggiungere il `scm` token per il nome dell'app web.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="6bbc0-285">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6bbc0-285">For example:</span></span>

| <span data-ttu-id="6bbc0-286">URL</span><span class="sxs-lookup"><span data-stu-id="6bbc0-286">URL</span></span>                                    | <span data-ttu-id="6bbc0-287">Risultato</span><span class="sxs-lookup"><span data-stu-id="6bbc0-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="6bbc0-288">App Web</span><span class="sxs-lookup"><span data-stu-id="6bbc0-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="6bbc0-289">Servizio kudu</span><span class="sxs-lookup"><span data-stu-id="6bbc0-289">Kudu service</span></span> |

<span data-ttu-id="6bbc0-290">Selezionare il [Console di Debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) voce di menu per visualizzare, modificare, eliminare o aggiungere i file.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6bbc0-291">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6bbc0-291">Additional resources</span></span>

* <span data-ttu-id="6bbc0-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) semplifica la distribuzione di App web e siti Web ai server IIS.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="6bbc0-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): I problemi di file e richiedere le funzionalità per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6bbc0-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
