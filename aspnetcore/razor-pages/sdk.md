---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 81025c14ba68971ca5d3cfc9387c2f50dd247654
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017404"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="3a418-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="3a418-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="3a418-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3a418-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="3a418-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3a418-105">Overview</span></span>

<span data-ttu-id="3a418-106">Il [!INCLUDE[](~/includes/2.1-SDK.md)] include il `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK di Razor).</span><span class="sxs-lookup"><span data-stu-id="3a418-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="3a418-107">Il Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="3a418-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="3a418-108">È necessario per compilare, creare un pacchetto e pubblicare progetti contenenti file [Razor](xref:mvc/views/razor) per ASP.NET Core progetti basati su MVC o [Blazer](xref:blazor/index) .</span><span class="sxs-lookup"><span data-stu-id="3a418-108">Is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based or [Blazor](xref:blazor/index) projects.</span></span>
* <span data-ttu-id="3a418-109">Include un set di destinazioni, proprietà ed elementi predefiniti che consentono di personalizzare la compilazione dei file Razor ( *. cshtml* o *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="3a418-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (*.cshtml* or *.razor*) files.</span></span>

<span data-ttu-id="3a418-110">Razor SDK include `Content` gli elementi con `Include` attributi impostati sui `**\*.cshtml` modelli glob `**\*.razor` e.</span><span class="sxs-lookup"><span data-stu-id="3a418-110">The Razor SDK includes `Content` items with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="3a418-111">I file corrispondenti vengono pubblicati.</span><span class="sxs-lookup"><span data-stu-id="3a418-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="3a418-112">Consente di standardizzare l'esperienza in termini di compilazione, creazione dei pacchetti e pubblicazione dei progetti contenenti file [Razor](xref:mvc/views/razor) per i progetti ASP.NET Core basati su MVC.</span><span class="sxs-lookup"><span data-stu-id="3a418-112">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="3a418-113">Include un set di destinazioni, proprietà e di elementi predefiniti che consentono di personalizzare la compilazione dei file Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-113">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

<span data-ttu-id="3a418-114">Razor SDK include un `Content` elemento con un `Include` attributo impostato sul `**\*.cshtml` modello glob.</span><span class="sxs-lookup"><span data-stu-id="3a418-114">The Razor SDK includes a `Content` item with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="3a418-115">I file corrispondenti vengono pubblicati.</span><span class="sxs-lookup"><span data-stu-id="3a418-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="3a418-116">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3a418-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="3a418-117">Usare Razor SDK</span><span class="sxs-lookup"><span data-stu-id="3a418-117">Use the Razor SDK</span></span>

<span data-ttu-id="3a418-118">La maggior parte delle App web non è necessario fare riferimento esplicitamente al SDK di Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3a418-119">Per usare Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages, è consigliabile iniziare con il modello di progetto libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="3a418-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages, we recommend starting with the Razor class library (RCL) project template.</span></span> <span data-ttu-id="3a418-120">Un RCL usato per compilare file blazer (*Razor*) richiede almeno un riferimento al pacchetto [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) .</span><span class="sxs-lookup"><span data-stu-id="3a418-120">An RCL that's used to build Blazor (*.razor*) files minimally requires a reference to the [Microsoft.AspNetCore.Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) package.</span></span> <span data-ttu-id="3a418-121">Un RCL usato per compilare visualizzazioni o pagine Razor (file con*estensione cshtml* ) richiede almeno la destinazione `netcoreapp3.0` o una versione successiva e dispone di un oggetto `FrameworkReference` nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="3a418-121">An RCL that's used to build Razor views or pages (*.cshtml* files) minimally requires targeting `netcoreapp3.0` or later and has a `FrameworkReference` to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in its project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3a418-122">Per usare il Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="3a418-122">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="3a418-123">Usare `Microsoft.NET.Sdk.Razor` anziché `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="3a418-123">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="3a418-124">In genere, un riferimento al pacchetto `Microsoft.AspNetCore.Mvc` è obbligatoria per ricevere le dipendenze aggiuntive necessarie per generare e compilare le pagine Razor e le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-124">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="3a418-125">Come minimo, il progetto deve aggiungere i riferimenti ai pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="3a418-125">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="3a418-126">Il `Microsoft.AspNetCore.Razor.Design` pacchetto fornisce l'attività di compilazione Razor e delle destinazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="3a418-126">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="3a418-127">I pacchetti precedenti sono inclusi in `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="3a418-127">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="3a418-128">Il markup seguente illustra un file di progetto che usa il SDK di Razor per generare i file Razor per un'app ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="3a418-128">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="3a418-129">Il `Microsoft.AspNetCore.Razor.Design` e `Microsoft.AspNetCore.Mvc.Razor.Extensions` i pacchetti sono inclusi nel [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3a418-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="3a418-130">Tuttavia, la versione l'uzo `Microsoft.AspNetCore.App` riferimento al pacchetto fornisce un metapacchetto all'app che non include la versione più recente di `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="3a418-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="3a418-131">I progetti devono fare riferimento a una versione coerente del `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) in modo che siano incluse le correzioni più recenti in fase di compilazione per Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="3a418-132">Per altre informazioni, vedere [questo problema su GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="3a418-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="3a418-133">Proprietà</span><span class="sxs-lookup"><span data-stu-id="3a418-133">Properties</span></span>

<span data-ttu-id="3a418-134">Le proprietà seguenti controllano il comportamento del Razor SDK come parte della compilazione del progetto:</span><span class="sxs-lookup"><span data-stu-id="3a418-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="3a418-135">`RazorCompileOnBuild` &ndash; Quando `true`, compila e crea l'assembly di Razor come parte della compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="3a418-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="3a418-136">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="3a418-136">Defaults to `true`.</span></span>
* <span data-ttu-id="3a418-137">`RazorCompileOnPublish` &ndash; Quando `true`, compila e crea l'assembly di Razor come parte della pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="3a418-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="3a418-138">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="3a418-138">Defaults to `true`.</span></span>

<span data-ttu-id="3a418-139">Le proprietà e gli elementi nella tabella seguente vengono utilizzati per configurare input e output per il SDK di Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="3a418-140">A partire da ASP.NET Core 3,0, le visualizzazioni MVC o Razor Pages non vengono gestite per `RazorCompileOnBuild` impostazione `RazorCompileOnPublish` predefinita se le proprietà o MSBuild nel file di progetto sono disabilitate.</span><span class="sxs-lookup"><span data-stu-id="3a418-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages aren't served by default if the `RazorCompileOnBuild` or `RazorCompileOnPublish` MSBuild properties in the project file are disabled.</span></span> <span data-ttu-id="3a418-141">Le applicazioni devono aggiungere un riferimento esplicito al pacchetto [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) se l'app si basa sulla compilazione di runtime per elaborare i file con *estensione cshtml* .</span><span class="sxs-lookup"><span data-stu-id="3a418-141">Applications must add an explicit reference to the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) package if the app relies on runtime compilation to process *.cshtml* files.</span></span>

::: moniker-end

| <span data-ttu-id="3a418-142">Elementi</span><span class="sxs-lookup"><span data-stu-id="3a418-142">Items</span></span> | <span data-ttu-id="3a418-143">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3a418-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="3a418-144">Elementi Item (file con*estensione cshtml* ) che sono input per la generazione di codice.</span><span class="sxs-lookup"><span data-stu-id="3a418-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="3a418-145">Elementi elemento (file*Razor* ) che sono input per la generazione di codice del componente Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-145">Item elements (*.razor* files) that are inputs to Razor component code generation.</span></span> |
| `RazorCompile` | <span data-ttu-id="3a418-146">Elementi Item (file con*estensione cs* ) che sono input per le destinazioni di compilazione Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="3a418-147">Usare questa `ItemGroup` impostazione per specificare i file aggiuntivi da compilare nell'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="3a418-148">Elementi della voce usati per attributi di generazione del codice per l'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="3a418-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3a418-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="3a418-150">Elementi aggiunti come risorse incorporate nell'assembly generato Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="3a418-151">Proprietà</span><span class="sxs-lookup"><span data-stu-id="3a418-151">Property</span></span> | <span data-ttu-id="3a418-152">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3a418-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="3a418-153">Nome di file (senza estensione) dell'assembly generato da Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="3a418-154">Directory di output di Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="3a418-155">Usato per determinare il set di strumenti utilizzato per compilare l'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="3a418-156">I valori validi sono `Implicit`, `RazorSDK` e `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="3a418-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="3a418-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="3a418-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="3a418-158">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="3a418-158">Default is `true`.</span></span> <span data-ttu-id="3a418-159">Quando `true`, include i file *Web. config*, *. JSON*e *. cshtml* come contenuto nel progetto.</span><span class="sxs-lookup"><span data-stu-id="3a418-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="3a418-160">Quando viene fatto riferimento tramite `Microsoft.NET.Sdk.Web`, i file sotto *wwwroot* e sono inclusi anche i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="3a418-161">Quando è `true`, include i file *cshtml* degli elementi di `Content` negli elementi di `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="3a418-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="3a418-162">Quando `true`, genera una *. cs* file che contiene gli attributi specificati da `RazorAssemblyAttribute` e include il file nell'output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="3a418-163">Quando è `true`, aggiunge un set predefinito di attributi degli assembly a `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="3a418-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="3a418-164">Quando `true`, le copie `RazorGenerate` gli elementi (*cshtml*) file alla directory di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="3a418-165">In genere, i file Razor non sono necessari per un'app pubblicata se partecipano alla compilazione in fase di compilazione o in fase di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="3a418-166">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="3a418-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="3a418-167">Quando è `true`, copiare gli elementi dell'assembly di riferimento nella directory di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="3a418-168">In genere, gli assembly di riferimento non sono necessari per un'app pubblicata se compilazione Razor si verifica in fase di compilazione o in fase di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="3a418-169">Impostare su `true` se l'app pubblicata richiede la compilazione di runtime.</span><span class="sxs-lookup"><span data-stu-id="3a418-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="3a418-170">Ad esempio, impostare il valore su `true` se l'app modifica *. cshtml* file in fase di esecuzione o utilizza visualizzazioni incorporate.</span><span class="sxs-lookup"><span data-stu-id="3a418-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="3a418-171">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="3a418-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="3a418-172">Quando `true`, tutti gli elementi di contenuto di Razor (*cshtml* file) sono contrassegnati per l'inclusione nel pacchetto NuGet generato.</span><span class="sxs-lookup"><span data-stu-id="3a418-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="3a418-173">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="3a418-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="3a418-174">Quando è `true`, aggiunge gli elementi (*cshtml*) di RazorGenerate come file incorporati nell'assembly Razor generato.</span><span class="sxs-lookup"><span data-stu-id="3a418-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="3a418-175">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="3a418-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="3a418-176">Quando è `true`, usa un processo del server di compilazione permanente per ripartire il lavoro di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="3a418-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="3a418-177">Valore predefinito è il valore di `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="3a418-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="3a418-178">Quando `true`, l'SDK genera attributi aggiuntivi usati da MVC in fase di esecuzione per eseguire l'individuazione delle parti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="3a418-179">Per altre informazioni sulle proprietà, vedere [Proprietà di MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="3a418-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="3a418-180">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="3a418-180">Targets</span></span>

<span data-ttu-id="3a418-181">Il Razor SDK definisce due obiettivi principali:</span><span class="sxs-lookup"><span data-stu-id="3a418-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="3a418-182">`RazorGenerate` &ndash; Genera codice *cs* dei file dalla `RazorGenerate` elementi item.</span><span class="sxs-lookup"><span data-stu-id="3a418-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="3a418-183">Utilizzare la `RazorGenerateDependsOn` proprietà per specificare destinazioni aggiuntive che possono essere eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-183">Use the `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="3a418-184">`RazorCompile` &ndash; Le compilazioni generate *cs* i file un assembly di Razor.</span><span class="sxs-lookup"><span data-stu-id="3a418-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="3a418-185">`RazorCompileDependsOn` Usare per specificare destinazioni aggiuntive che possono essere eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-185">Use the `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="3a418-186">`RazorComponentGenerate`Il codice genera file con *estensione cs* per `RazorComponent` gli elementi Item. &ndash;</span><span class="sxs-lookup"><span data-stu-id="3a418-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="3a418-187">Utilizzare la `RazorComponentGenerateDependsOn` proprietà per specificare destinazioni aggiuntive che possono essere eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-187">Use the `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="3a418-188">Compilazione runtime di visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="3a418-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="3a418-189">Per impostazione predefinita, Razor SDK non pubblica gli assembly di riferimento necessari per eseguire la compilazione runtime.</span><span class="sxs-lookup"><span data-stu-id="3a418-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="3a418-190">Vengono pertanto generati errori di compilazione nel caso in cui il modello di applicazione si basi su una compilazione runtime&mdash;ad esempio, l'app usa visualizzazioni incorporate o modifica le visualizzazioni dopo aver pubblicato l'app.</span><span class="sxs-lookup"><span data-stu-id="3a418-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="3a418-191">Impostare `CopyRefAssembliesToPublishDirectory` su `true` per continuare con la pubblicazione degli assembly di riferimento.</span><span class="sxs-lookup"><span data-stu-id="3a418-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="3a418-192">Per un'app web, assicurarsi che l'app è destinata al `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="3a418-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="3a418-193">Versione della lingua Razor</span><span class="sxs-lookup"><span data-stu-id="3a418-193">Razor language version</span></span>

<span data-ttu-id="3a418-194">Quando la destinazione `Microsoft.NET.Sdk.Web` è SDK, la versione della lingua Razor viene dedotta dalla versione del Framework di destinazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="3a418-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="3a418-195">Per i progetti destinati all' `Microsoft.NET.Sdk.Razor` SDK o nel raro caso in cui l'app richieda una versione diversa della lingua Razor rispetto al valore derivato, è possibile configurare una versione impostando la `<RazorLangVersion>` proprietà nel file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="3a418-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="3a418-196">La versione del linguaggio Razor è strettamente integrata con la versione del runtime per cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="3a418-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="3a418-197">La destinazione di una versione di linguaggio non progettata per il runtime non è supportata e probabilmente genera errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="3a418-197">Targeting a language version that isn't designed for the runtime is unsupported and likely produces build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3a418-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3a418-198">Additional resources</span></span>

* [<span data-ttu-id="3a418-199">Aggiunte al formato csproj per .NET Core</span><span class="sxs-lookup"><span data-stu-id="3a418-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="3a418-200">Elementi di progetto MSBuild comuni</span><span class="sxs-lookup"><span data-stu-id="3a418-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
