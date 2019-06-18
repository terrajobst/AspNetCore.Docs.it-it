---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196367"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="cbff9-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="cbff9-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="cbff9-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cbff9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="cbff9-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="cbff9-105">Overview</span></span>

<span data-ttu-id="cbff9-106">Il [!INCLUDE[](~/includes/2.1-SDK.md)] include il `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK di Razor).</span><span class="sxs-lookup"><span data-stu-id="cbff9-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="cbff9-107">Il Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="cbff9-107">The Razor SDK:</span></span>

* <span data-ttu-id="cbff9-108">Consente di standardizzare l'esperienza in termini di compilazione, creazione dei pacchetti e pubblicazione dei progetti contenenti file [Razor](xref:mvc/views/razor) per i progetti ASP.NET Core basati su MVC.</span><span class="sxs-lookup"><span data-stu-id="cbff9-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="cbff9-109">Include un set di destinazioni, proprietà e di elementi predefiniti che consentono di personalizzare la compilazione dei file Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="cbff9-110">il SDK di Razor include un' `<Content>` elemento con un `Include` attributo impostato sul `**\*.cshtml` criterio glob.</span><span class="sxs-lookup"><span data-stu-id="cbff9-110">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="cbff9-111">I file corrispondenti vengono pubblicati.</span><span class="sxs-lookup"><span data-stu-id="cbff9-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cbff9-112">Include il SDK di Razor `<Content>` elementi con `Include` impostare gli attributi per il `**\*.cshtml` e `**\*.razor` glob.</span><span class="sxs-lookup"><span data-stu-id="cbff9-112">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="cbff9-113">I file corrispondenti vengono pubblicati.</span><span class="sxs-lookup"><span data-stu-id="cbff9-113">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="cbff9-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cbff9-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="cbff9-115">Usare il SDK di Razor</span><span class="sxs-lookup"><span data-stu-id="cbff9-115">Use the Razor SDK</span></span>

<span data-ttu-id="cbff9-116">La maggior parte delle App web non è necessario fare riferimento esplicitamente al SDK di Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-116">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="cbff9-117">Per usare il Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="cbff9-117">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="cbff9-118">Usare `Microsoft.NET.Sdk.Razor` anziché `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="cbff9-118">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="cbff9-119">In genere, un riferimento al pacchetto `Microsoft.AspNetCore.Mvc` è obbligatoria per ricevere le dipendenze aggiuntive necessarie per generare e compilare le pagine Razor e le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-119">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="cbff9-120">Come minimo, il progetto deve aggiungere i riferimenti ai pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="cbff9-120">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="cbff9-121">Il `Microsoft.AspNetCore.Razor.Design` pacchetto fornisce l'attività di compilazione Razor e delle destinazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="cbff9-121">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="cbff9-122">I pacchetti precedenti sono inclusi in `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-122">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="cbff9-123">Il markup seguente illustra un file di progetto che usa il SDK di Razor per generare i file Razor per un'app ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="cbff9-123">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="cbff9-124">Il `Microsoft.AspNetCore.Razor.Design` e `Microsoft.AspNetCore.Mvc.Razor.Extensions` i pacchetti sono inclusi nel [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="cbff9-124">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="cbff9-125">Tuttavia, la versione l'uzo `Microsoft.AspNetCore.App` riferimento al pacchetto fornisce un metapacchetto all'app che non include la versione più recente di `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-125">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="cbff9-126">I progetti devono fare riferimento a una versione coerente del `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) in modo che siano incluse le correzioni più recenti in fase di compilazione per Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-126">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="cbff9-127">Per altre informazioni, vedere [questo problema su GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="cbff9-127">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="cbff9-128">Proprietà</span><span class="sxs-lookup"><span data-stu-id="cbff9-128">Properties</span></span>

<span data-ttu-id="cbff9-129">Le proprietà seguenti controllano il comportamento del Razor SDK come parte della compilazione del progetto:</span><span class="sxs-lookup"><span data-stu-id="cbff9-129">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="cbff9-130">`RazorCompileOnBuild` &ndash; Quando `true`, compila e crea l'assembly di Razor come parte della compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="cbff9-130">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="cbff9-131">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-131">Defaults to `true`.</span></span>
* <span data-ttu-id="cbff9-132">`RazorCompileOnPublish` &ndash; Quando `true`, compila e crea l'assembly di Razor come parte della pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="cbff9-132">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="cbff9-133">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-133">Defaults to `true`.</span></span>

<span data-ttu-id="cbff9-134">Le proprietà e gli elementi nella tabella seguente vengono utilizzati per configurare input e output per il SDK di Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-134">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="cbff9-135">Elementi</span><span class="sxs-lookup"><span data-stu-id="cbff9-135">Items</span></span> | <span data-ttu-id="cbff9-136">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cbff9-136">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="cbff9-137">Elementi della voce (file *cshtml*) che sono input per le destinazioni di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="cbff9-137">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="cbff9-138">Elementi Item (*cs* file) che sono input per le destinazioni di compilazione Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-138">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="cbff9-139">Utilizzare questa opzione `ItemGroup` per specificare i file aggiuntivi da compilare in assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-139">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="cbff9-140">Elementi della voce usati per attributi di generazione del codice per l'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-140">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="cbff9-141">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cbff9-141">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="cbff9-142">Elementi aggiunti come risorse incorporate nell'assembly generato Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-142">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="cbff9-143">Proprietà</span><span class="sxs-lookup"><span data-stu-id="cbff9-143">Property</span></span> | <span data-ttu-id="cbff9-144">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cbff9-144">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="cbff9-145">Nome di file (senza estensione) dell'assembly generato da Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-145">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="cbff9-146">Directory di output di Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-146">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="cbff9-147">Usato per determinare il set di strumenti utilizzato per compilare l'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-147">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="cbff9-148">I valori validi sono `Implicit`, `RazorSDK` e `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-148">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="cbff9-149">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="cbff9-149">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="cbff9-150">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-150">Default is `true`.</span></span> <span data-ttu-id="cbff9-151">Quando `true`, include *Web. config*, *JSON*, e *cshtml* file come contenuto del progetto.</span><span class="sxs-lookup"><span data-stu-id="cbff9-151">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="cbff9-152">Quando viene fatto riferimento tramite `Microsoft.NET.Sdk.Web`, i file sotto *wwwroot* e sono inclusi anche i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-152">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="cbff9-153">Quando è `true`, include i file *cshtml* degli elementi di `Content` negli elementi di `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-153">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="cbff9-154">Quando `true`, genera una *. cs* file che contiene gli attributi specificati da `RazorAssemblyAttribute` e include il file nell'output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-154">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="cbff9-155">Quando è `true`, aggiunge un set predefinito di attributi degli assembly a `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-155">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="cbff9-156">Quando `true`, le copie `RazorGenerate` gli elementi (*cshtml*) file alla directory di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-156">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="cbff9-157">In genere, i file Razor non sono necessari per un'app pubblicata se partecipano alla compilazione in fase di compilazione o in fase di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-157">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="cbff9-158">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-158">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="cbff9-159">Quando è `true`, copiare gli elementi dell'assembly di riferimento nella directory di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-159">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="cbff9-160">In genere, gli assembly di riferimento non sono necessari per un'app pubblicata se compilazione Razor si verifica in fase di compilazione o in fase di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-160">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="cbff9-161">Impostare su `true` se l'app pubblicata richiede la compilazione di runtime.</span><span class="sxs-lookup"><span data-stu-id="cbff9-161">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="cbff9-162">Ad esempio, impostare il valore su `true` se l'app modifica *. cshtml* file in fase di esecuzione o utilizza visualizzazioni incorporate.</span><span class="sxs-lookup"><span data-stu-id="cbff9-162">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="cbff9-163">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-163">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="cbff9-164">Quando `true`, tutti gli elementi di contenuto di Razor (*cshtml* file) sono contrassegnati per l'inclusione nel pacchetto NuGet generato.</span><span class="sxs-lookup"><span data-stu-id="cbff9-164">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="cbff9-165">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-165">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="cbff9-166">Quando è `true`, aggiunge gli elementi (*cshtml*) di RazorGenerate come file incorporati nell'assembly Razor generato.</span><span class="sxs-lookup"><span data-stu-id="cbff9-166">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="cbff9-167">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-167">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="cbff9-168">Quando è `true`, usa un processo del server di compilazione permanente per ripartire il lavoro di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="cbff9-168">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="cbff9-169">Valore predefinito è il valore di `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="cbff9-169">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="cbff9-170">Per altre informazioni sulle proprietà, vedere [Proprietà di MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="cbff9-170">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="cbff9-171">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="cbff9-171">Targets</span></span>

<span data-ttu-id="cbff9-172">Il Razor SDK definisce due obiettivi principali:</span><span class="sxs-lookup"><span data-stu-id="cbff9-172">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="cbff9-173">`RazorGenerate` &ndash; Genera codice *cs* dei file dalla `RazorGenerate` elementi item.</span><span class="sxs-lookup"><span data-stu-id="cbff9-173">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="cbff9-174">Usa la proprietà `RazorGenerateDependsOn` per specificare le altre destinazioni che possono essere eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-174">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="cbff9-175">`RazorCompile` &ndash; Le compilazioni generate *cs* i file un assembly di Razor.</span><span class="sxs-lookup"><span data-stu-id="cbff9-175">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="cbff9-176">Usare `RazorCompileDependsOn` per specificare le altre destinazioni che possono eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="cbff9-176">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="cbff9-177">Compilazione runtime di visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="cbff9-177">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="cbff9-178">Per impostazione predefinita, Razor SDK non pubblica gli assembly di riferimento necessari per eseguire la compilazione runtime.</span><span class="sxs-lookup"><span data-stu-id="cbff9-178">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="cbff9-179">Vengono pertanto generati errori di compilazione nel caso in cui il modello di applicazione si basi su una compilazione runtime&mdash;ad esempio, l'app usa visualizzazioni incorporate o modifica le visualizzazioni dopo aver pubblicato l'app.</span><span class="sxs-lookup"><span data-stu-id="cbff9-179">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="cbff9-180">Impostare `CopyRefAssembliesToPublishDirectory` su `true` per continuare con la pubblicazione degli assembly di riferimento.</span><span class="sxs-lookup"><span data-stu-id="cbff9-180">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="cbff9-181">Per un'app web, assicurarsi che l'app è destinata al `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="cbff9-181">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="cbff9-182">Versione del linguaggio Razor</span><span class="sxs-lookup"><span data-stu-id="cbff9-182">Razor language version</span></span>

<span data-ttu-id="cbff9-183">Quando la destinazione è il `Microsoft.NET.Sdk.Web` SDK, la versione del linguaggio Razor viene dedotto dalla versione di framework di destinazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="cbff9-183">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="cbff9-184">Per i progetti destinati al `Microsoft.NET.Sdk.Razor` SDK o nel raro caso che l'app richiede una versione di linguaggio Razor diversa rispetto al valore derivato, una versione può essere configurata impostando il `<RazorLangVersion>` proprietà nel file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="cbff9-184">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a><span data-ttu-id="cbff9-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cbff9-185">Additional resources</span></span>

* [<span data-ttu-id="cbff9-186">Aggiunte al formato csproj per .NET Core</span><span class="sxs-lookup"><span data-stu-id="cbff9-186">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="cbff9-187">Elementi di progetto MSBuild comuni</span><span class="sxs-lookup"><span data-stu-id="cbff9-187">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
