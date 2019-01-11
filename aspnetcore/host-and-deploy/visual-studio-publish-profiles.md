---
title: Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core
author: rick-anderson
description: Informazioni su come creare profili di pubblicazione in Visual Studio e usarli per la gestione delle distribuzioni di app ASP.NET Core in varie destinazioni.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3d24cd2cd4697e8e7cf7e4bdf4d076a09b6a6a23
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284708"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="5cd9b-103">Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cd9b-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="5cd9b-104">Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5cd9b-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="5cd9b-105">Per la versione 1.1 di questo argomento, scaricare [Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core (versione 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-105">For the 1.1 version of this topic, download [Visual Studio publish profiles for ASP.NET Core app deployment (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="5cd9b-106">Questo documento descrive come usare Visual Studio 2017 o versioni successive per la creazione e l'uso di profili di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-106">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="5cd9b-107">I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="5cd9b-108">Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="5cd9b-109">Il file di progetto seguente è stato creato con il comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-109">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

<span data-ttu-id="5cd9b-110">L'attributo `Sdk` dell'elemento `<Project>` esegue le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-110">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="5cd9b-111">Importa il file delle proprietà da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-111">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="5cd9b-112">Importa il file di destinazioni da  *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-112">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="5cd9b-113">Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-113">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="5cd9b-114">L'SDK `Microsoft.NET.Sdk.Web` dipende da:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-114">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="5cd9b-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="5cd9b-116">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-116">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="5cd9b-117">Che determina l'importazione delle proprietà e destinazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-117">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="5cd9b-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="5cd9b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="5cd9b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="5cd9b-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="5cd9b-122">Le destinazioni di pubblicazione importano il set corretto di destinazioni in base al metodo di pubblicazione usato.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-122">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="5cd9b-123">Quando MSBuild o Visual Studio carica un progetto, vengono eseguite le azioni di alto livello seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-123">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="5cd9b-124">Compilare un progetto</span><span class="sxs-lookup"><span data-stu-id="5cd9b-124">Build project</span></span>
* <span data-ttu-id="5cd9b-125">Calcolare i file da pubblicare</span><span class="sxs-lookup"><span data-stu-id="5cd9b-125">Compute files to publish</span></span>
* <span data-ttu-id="5cd9b-126">Pubblicare i file nella destinazione</span><span class="sxs-lookup"><span data-stu-id="5cd9b-126">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="5cd9b-127">Calcolare gli elementi del progetto</span><span class="sxs-lookup"><span data-stu-id="5cd9b-127">Compute project items</span></span>

<span data-ttu-id="5cd9b-128">Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-128">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="5cd9b-129">L'attributo `item type` determina la modalità di elaborazione del file.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-129">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="5cd9b-130">Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-130">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="5cd9b-131">I file presenti nell'elenco di elementi `Compile` vengono compilati.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-131">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="5cd9b-132">L'elenco di elementi `Content` contiene i file che sono pubblicati in aggiunta agli output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-132">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="5cd9b-133">Per impostazione predefinita, i file corrispondenti al criterio `wwwroot/**` saranno inclusi nell'elemento `Content`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-133">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="5cd9b-134">Il [criterio GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot/\*\*` specifica tutti i file nella cartella *wwwroot* **e** relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-134">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="5cd9b-135">Per aggiungere esplicitamente un file all'elenco di pubblicazione, aggiungere il file direttamente nel file *.csproj*, come illustrato in [File di inclusione](#include-files).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-135">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="5cd9b-136">Quando si seleziona il pulsante **Pubblica** in Visual Studio o quando si pubblica dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-136">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="5cd9b-137">Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-137">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="5cd9b-138">**Solo Visual Studio**: i pacchetti NuGet vengono ripristinati.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-138">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="5cd9b-139">(Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)</span><span class="sxs-lookup"><span data-stu-id="5cd9b-139">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="5cd9b-140">Il progetto viene compilato.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-140">The project builds.</span></span>
* <span data-ttu-id="5cd9b-141">Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-141">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="5cd9b-142">Il progetto viene pubblicato (i file calcolati vengono copiati nella destinazione di pubblicazione).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-142">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="5cd9b-143">Quando un progetto ASP.NET Core fa riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto, nella radice della directory dell'app Web viene posizionato un file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-143">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="5cd9b-144">Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-144">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="5cd9b-145">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-145">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="5cd9b-146">Pubblicazione di base dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="5cd9b-146">Basic command-line publishing</span></span>

<span data-ttu-id="5cd9b-147">La pubblicazione dalla riga di comando funziona su tutte le piattaforme supportate da .NET Core e non richiede Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-147">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="5cd9b-148">Negli esempi seguenti, il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) viene eseguito dalla directory del progetto (che contiene il file *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-148">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="5cd9b-149">Se non si è nella cartella del progetto, passare esplicitamente il percorso del file del progetto.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-149">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="5cd9b-150">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-150">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="5cd9b-151">Eseguire i comandi seguenti per creare e pubblicare un'app Web:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-151">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="5cd9b-152">Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) genera un output analogo a quello illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-152">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="5cd9b-153">La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-153">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="5cd9b-154">Il valore predefinito per `$(Configuration)` è *Debug*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-154">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="5cd9b-155">Nell'esempio precedente, `<TargetFramework>` è `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-155">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="5cd9b-156">`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="5cd9b-157">Il comando seguente specifica un build `Release` e la directory di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="5cd9b-158">Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) chiama MSBuild, che richiama la destinazione `Publish`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-158">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="5cd9b-159">Tutti i parametri passati a `dotnet publish` vengono passati a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-159">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="5cd9b-160">Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="5cd9b-161">Il parametro `-o` esegue il mapping a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="5cd9b-162">È possibile passare le proprietà di MSBuild usando uno dei formati seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-162">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="5cd9b-163">Il comando seguente pubblica un build `Release` a una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="5cd9b-164">La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="5cd9b-165">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="5cd9b-166">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="5cd9b-167">La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="5cd9b-168">Profili di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="5cd9b-168">Publish profiles</span></span>

<span data-ttu-id="5cd9b-169">Questa sezione usa Visual Studio 2017 o versioni successive per creare un profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-169">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="5cd9b-170">Dopo la creazione del profilo, la pubblicazione è disponibile da Visual Studio o dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-170">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="5cd9b-171">I profili di pubblicazione possono semplificare il processo di pubblicazione e può essere presente un numero illimitato di profili.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-171">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="5cd9b-172">Creare un profilo di pubblicazione in Visual Studio scegliendo uno dei seguenti percorsi:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-172">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="5cd9b-173">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-173">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="5cd9b-174">Selezionare **Pubblica {NOME PROGETTO}** dal menu **Compila**.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-174">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="5cd9b-175">Verrà visualizzata la scheda **Pubblica** della pagina di capacità dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-175">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="5cd9b-176">Se il progetto non dispone di un profilo di pubblicazione, viene visualizzata la pagina seguente:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-176">If the project lacks a publish profile, the following page is displayed:</span></span>

![Scheda Pubblica della pagina di capacità dell'app](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="5cd9b-178">Quando è selezionata l'opzione **Cartella**, specificare il percorso di una cartella in cui archiviare gli asset pubblicati.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-178">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="5cd9b-179">La cartella predefinita è *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-179">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="5cd9b-180">Fare clic sul pulsante **Crea profilo** per terminare.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-180">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="5cd9b-181">Dopo aver creato un profilo di pubblicazione, la scheda **Pubblica** cambia.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-181">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="5cd9b-182">Il nuovo profilo creato viene visualizzato in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="5cd9b-183">Fare clic su **Crea nuovo profilo** per creare un altro nuovo profilo.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-183">Click **Create new profile** to create another new profile.</span></span>

![Scheda Pubblica della pagina di capacità dell'app che visualizza FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="5cd9b-185">La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-185">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="5cd9b-186">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="5cd9b-186">Azure App Service</span></span>
* <span data-ttu-id="5cd9b-187">Macchine virtuali di Azure</span><span class="sxs-lookup"><span data-stu-id="5cd9b-187">Azure Virtual Machines</span></span>
* <span data-ttu-id="5cd9b-188">IIS, FTP e così via (per qualunque server Web)</span><span class="sxs-lookup"><span data-stu-id="5cd9b-188">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="5cd9b-189">Cartella</span><span class="sxs-lookup"><span data-stu-id="5cd9b-189">Folder</span></span>
* <span data-ttu-id="5cd9b-190">Importa profilo</span><span class="sxs-lookup"><span data-stu-id="5cd9b-190">Import Profile</span></span>

<span data-ttu-id="5cd9b-191">Per altre informazioni, vedere [Quali sono le opzioni di pubblicazione più adatte?](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-191">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="5cd9b-192">Quando si crea un profilo di pubblicazione con Visual Studio, viene creato un file di MSBuild *Properties/PublishProfiles/{NOME PROFILO}.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-192">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="5cd9b-193">Il file *.pubxml* è un file di MSBuild e contiene le impostazioni di configurazione della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-193">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="5cd9b-194">Questo file può essere modificato per personalizzare il processo di compilazione e di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-194">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="5cd9b-195">Questo file viene letto dal processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-195">This file is read by the publishing process.</span></span> <span data-ttu-id="5cd9b-196">`<LastUsedBuildConfiguration>` è speciale perché è una proprietà globale e non deve essere presente in alcun file importato nella compilazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-196">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="5cd9b-197">Per altre informazioni, vedere [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: come impostare la proprietà di configurazione).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-197">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="5cd9b-198">Quando si esegue la pubblicazione in una destinazione di Azure, il file *.pubxml* contiene l'identificatore della sottoscrizione Azure.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-198">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="5cd9b-199">Con tale tipo di destinazione, è sconsigliato aggiungere questo file al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-199">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="5cd9b-200">Quando si pubblica in una destinazione non Azure, è possibile archiviare il file *.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-200">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="5cd9b-201">Le informazioni riservate, come la password di pubblicazione, vengono crittografate a livello di singolo utente/computer.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-201">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="5cd9b-202">Sono memorizzate nel file *Properties/PublishProfiles/{NOME PROFILO}.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-202">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="5cd9b-203">Poiché questo file può contenere informazioni riservate, non deve essere archiviato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-203">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="5cd9b-204">Per una panoramica su come pubblicare un'app Web in ASP.NET Core, vedere [Ospitare e distribuire](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-204">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="5cd9b-205">Le attività e le destinazioni di MSBuild necessarie per pubblicare un'app ASP.NET Core sono open source in https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-205">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="5cd9b-206">`dotnet publish` può usare profili di pubblicazione di tipo cartella, MSDeploy e [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="5cd9b-206">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="5cd9b-207">Cartella (multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="5cd9b-207">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="5cd9b-208">MSDeploy (attualmente funziona solo in Windows poiché MSDeploy non è multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="5cd9b-208">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="5cd9b-209">Pacchetto MSDeploy (attualmente funziona solo in Windows poiché MSDeploy non è multipiattaforma):</span><span class="sxs-lookup"><span data-stu-id="5cd9b-209">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="5cd9b-210">Negli esempi precedenti **non** passare `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-210">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="5cd9b-211">Per altre informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-211">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="5cd9b-212">`dotnet publish` supporta le API Kudu per la pubblicazione in Azure da qualsiasi piattaforma.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-212">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="5cd9b-213">La pubblicazione con Visual Studio supporta le API Kudu, ma è supportata da WebSDK per la pubblicazione multipiattaforma in Azure.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-213">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="5cd9b-214">Aggiungere un profilo di pubblicazione alla cartella *Properties/PublishProfiles* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-214">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="5cd9b-215">Eseguire il comando seguente per comprimere il contenuto per la pubblicazione e pubblicarlo in Azure tramite le API Kudu:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-215">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="5cd9b-216">Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-216">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="5cd9b-217">Durante la pubblicazione con un profilo denominato *FolderProfile*, è possibile eseguire uno dei comandi riportati di seguito:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-217">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="5cd9b-218">Richiamando [dotnet build](/dotnet/core/tools/dotnet-build), viene chiamato `msbuild` per eseguire il processo di compilazione e di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-218">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="5cd9b-219">Quando si passa un profilo di cartella, è equivalente chiamare `dotnet build` o `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-219">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="5cd9b-220">Chiamando MSBuild direttamente in Windows, viene usata la versione .NET Framework di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-220">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="5cd9b-221">MSDeploy è attualmente limitato ai computer Windows per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-221">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="5cd9b-222">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-222">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="5cd9b-223">Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-223">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="5cd9b-224">Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-224">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="5cd9b-225">Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-225">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="5cd9b-226">Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-226">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="5cd9b-227">Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-227">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="5cd9b-228">La proprietà di configurazione `<LastUsedBuildConfiguration>` è speciale e non deve essere sottoposta a override in un file di MSBuild importato.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-228">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="5cd9b-229">È possibile eseguire l'override di questa proprietà dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-229">This property can be overridden from the command line.</span></span>

<span data-ttu-id="5cd9b-230">Mediante l'interfaccia della riga di comando di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-230">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="5cd9b-231">Mediante MSBuild:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-231">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="5cd9b-232">Pubblicare in un endpoint di MSDeploy dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="5cd9b-232">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="5cd9b-233">L'esempio seguente usa un'app Web ASP.NET Core creata da Visual Studio e denominata *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-233">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="5cd9b-234">Un profilo di pubblicazione app di Azure viene aggiunto con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-234">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="5cd9b-235">Per altre informazioni su come creare un profilo, vedere la sezione [Profili di pubblicazione](#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-235">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="5cd9b-236">Per distribuire l'app usando un profilo di pubblicazione, eseguire il comando `msbuild` dal **prompt dei comandi per gli sviluppatori** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-236">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="5cd9b-237">Il prompt dei comandi è disponibile nella cartella *Visual Studio* del menu **Start** della barra delle applicazioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-237">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="5cd9b-238">Per semplificare l'accesso, è possibile aggiungere il prompt dei comandi al menu **Strumenti** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-238">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="5cd9b-239">Per altre informazioni, vedere [Prompt dei comandi per gli sviluppatori per Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-239">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="5cd9b-240">MSBuild usa la sintassi dei comandi seguente:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-240">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="5cd9b-241">{PERCORSO} &ndash; Percorso del file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-241">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="5cd9b-242">{PROFILO} &ndash; Nome del profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-242">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="5cd9b-243">{NOMEUTENTE} &ndash; Nome utente MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-243">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="5cd9b-244">{NOMEUTENTE} è disponibile nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-244">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="5cd9b-245">{PASSWORD} &ndash; Password di MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-245">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="5cd9b-246">Il valore {PASSWORD} è disponibile nel file *{PROFILO}.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-246">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="5cd9b-247">Scaricare il file *.PublishSettings* da:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-247">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="5cd9b-248">Esplora soluzioni: Selezionare **Visualizza** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-248">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="5cd9b-249">Connettersi alla sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-249">Connect with your Azure subscription.</span></span> <span data-ttu-id="5cd9b-250">Aprire **Servizi app**.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-250">Open **App Services**.</span></span> <span data-ttu-id="5cd9b-251">Fare clic con il pulsante destro del mouse sull'app.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-251">Right-click the app.</span></span> <span data-ttu-id="5cd9b-252">Selezionare **Scarica profilo di pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-252">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="5cd9b-253">Portale di Azure: selezionare **Recupera profilo di pubblicazione** nel pannello **Panoramica** dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-253">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="5cd9b-254">L'esempio seguente usa un profilo di pubblicazione denominato *AzureWebApp - Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-254">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="5cd9b-255">Un profilo di pubblicazione può essere usato anche con il comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) dell'interfaccia della riga di comando di .NET Core al prompt dei comandi di Windows:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-255">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="5cd9b-256">Il comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) è multipiattaforma e consente di compilare app ASP.NET Core in macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-256">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="5cd9b-257">Tuttavia, MSBuild in macOS e Linux non è in grado di distribuire un'app in Azure o in un altro endpoint MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-257">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="5cd9b-258">MSDeploy è disponibile solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-258">MSDeploy is only available on Windows.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="5cd9b-259">Escludere file</span><span class="sxs-lookup"><span data-stu-id="5cd9b-259">Exclude files</span></span>

<span data-ttu-id="5cd9b-260">Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-260">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="5cd9b-261">`msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-261">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="5cd9b-262">Ad esempio, l'elemento `<Content>` seguente esclude tutti i file di testo (*.txt*) dalla cartella *wwwroot/content* e da tutte le relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-262">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="5cd9b-263">Il markup precedente può essere aggiunto a un profilo di pubblicazione o al file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-263">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="5cd9b-264">Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-264">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="5cd9b-265">L'elemento `<MsDeploySkipRules>` seguente esclude tutti i file dalla cartella *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-265">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="5cd9b-266">`<MsDeploySkipRules>` non elimina le destinazioni da *ignorare* dal sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-266">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="5cd9b-267">Vengono eliminati dal sito di distribuzione i file e le cartelle di destinazione `<Content>`.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-267">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="5cd9b-268">Si supponga, ad esempio, che un'app Web distribuita contenga i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-268">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="5cd9b-269">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-269">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="5cd9b-270">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-270">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="5cd9b-271">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5cd9b-271">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="5cd9b-272">Se si aggiungono gli elementi `<MsDeploySkipRules>` seguenti, questi file non vengono eliminati nel sito di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-272">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="5cd9b-273">Gli elementi `<MsDeploySkipRules>` precedenti impediscono che i file *ignorati* vengano distribuiti.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-273">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="5cd9b-274">Tali file non verranno eliminati una volta distribuiti.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-274">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="5cd9b-275">L'elemento `<Content>` seguente elimina i file di destinazione sul sito di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-275">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="5cd9b-276">L'uso della distribuzione dalla riga di comando con l'elemento `<Content>` precedente produce il seguente output:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-276">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="5cd9b-277">File di inclusione</span><span class="sxs-lookup"><span data-stu-id="5cd9b-277">Include files</span></span>

<span data-ttu-id="5cd9b-278">Il markup seguente include una cartella *images* all'esterno della directory di progetto nella cartella *wwwroot/images* del sito di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-278">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="5cd9b-279">Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-279">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="5cd9b-280">Se viene aggiunto al file *.csproj*, è incluso in ogni profilo di pubblicazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-280">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="5cd9b-281">Il markup evidenziato seguente illustra come:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-281">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="5cd9b-282">Copiare un file dall'esterno del progetto nella cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-282">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="5cd9b-283">Escludere la cartella *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-283">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="5cd9b-284">Escludere *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-284">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="5cd9b-285">Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-285">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="5cd9b-286">Eseguire una destinazione prima o dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="5cd9b-286">Run a target before or after publishing</span></span>

<span data-ttu-id="5cd9b-287">Le destinazioni incorporate `BeforePublish` e `AfterPublish` consentono di eseguire una destinazione prima o dopo la destinazione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-287">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="5cd9b-288">Aggiungere gli elementi seguenti al profilo di pubblicazione per registrare i messaggi della console prima e dopo la pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-288">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="5cd9b-289">Pubblicazione in un server tramite un certificato non attendibile</span><span class="sxs-lookup"><span data-stu-id="5cd9b-289">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="5cd9b-290">Aggiungere la proprietà `<AllowUntrustedCertificate>` con un valore `True` al profilo di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-290">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="5cd9b-291">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="5cd9b-291">The Kudu service</span></span>

<span data-ttu-id="5cd9b-292">Per visualizzare i file in una distribuzione di app Web di Servizio app di Azure, usare il [servizio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="5cd9b-292">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="5cd9b-293">Accodare il token `scm` al nome dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-293">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="5cd9b-294">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5cd9b-294">For example:</span></span>

| <span data-ttu-id="5cd9b-295">URL</span><span class="sxs-lookup"><span data-stu-id="5cd9b-295">URL</span></span>                                    | <span data-ttu-id="5cd9b-296">Risultato</span><span class="sxs-lookup"><span data-stu-id="5cd9b-296">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="5cd9b-297">App Web</span><span class="sxs-lookup"><span data-stu-id="5cd9b-297">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="5cd9b-298">Servizio Kudu</span><span class="sxs-lookup"><span data-stu-id="5cd9b-298">Kudu service</span></span> |

<span data-ttu-id="5cd9b-299">Selezionare la voce di menu [Console di debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) per visualizzare, modificare, eliminare o aggiungere file.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-299">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5cd9b-300">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5cd9b-300">Additional resources</span></span>

* <span data-ttu-id="5cd9b-301">[Distribuzione Web](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) semplifica la distribuzione di app Web e siti Web sui server IIS.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-301">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="5cd9b-302">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemi di file e funzionalità di richiesta per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5cd9b-302">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="5cd9b-303">Pubblicare un'app Web ASP.NET in una macchina virtuale di Azure da Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5cd9b-303">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
