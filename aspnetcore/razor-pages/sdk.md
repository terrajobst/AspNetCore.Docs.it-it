---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: 1dc001c7c5fe320629835e06fe6db7fadabff94d
ms.sourcegitcommit: 6189b0ced9c115248c6ede02efcd0b29d31f2115
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985397"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="4e478-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="4e478-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="4e478-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4e478-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="4e478-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4e478-105">Overview</span></span>

<span data-ttu-id="4e478-106">Il [!INCLUDE[](~/includes/2.1-SDK.md)] include il `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK di Razor).</span><span class="sxs-lookup"><span data-stu-id="4e478-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="4e478-107">Il Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="4e478-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
* <span data-ttu-id="4e478-108">Consente di standardizzare l'esperienza in termini di compilazione, creazione dei pacchetti e pubblicazione dei progetti contenenti file [Razor](xref:mvc/views/razor) per i progetti ASP.NET Core basati su MVC.</span><span class="sxs-lookup"><span data-stu-id="4e478-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="4e478-109">Include un set di destinazioni, proprietà e di elementi predefiniti che consentono di personalizzare la compilazione dei file Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
* <span data-ttu-id="4e478-110">è necessario per la compilazione, il creazione di pacchetti e la pubblicazione di progetti contenenti file [Razor](xref:mvc/views/razor) per ASP.NET Core progetti basati su MVC o progetti [Blazer](xref:blazor/index)</span><span class="sxs-lookup"><span data-stu-id="4e478-110">is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects, or [Blazor](xref:blazor/index) projects</span></span>
* <span data-ttu-id="4e478-111">Include un set di destinazioni, proprietà ed elementi predefiniti che consentono di personalizzare la compilazione dei file Razor (. cshtml o. Razor).</span><span class="sxs-lookup"><span data-stu-id="4e478-111">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (.cshtml or .razor) files.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="4e478-112">Razor SDK include un `<Content>` elemento con un `Include` attributo impostato sul `**\*.cshtml` modello glob.</span><span class="sxs-lookup"><span data-stu-id="4e478-112">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="4e478-113">I file corrispondenti vengono pubblicati.</span><span class="sxs-lookup"><span data-stu-id="4e478-113">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4e478-114">Razor SDK include `<Content>` elementi con `Include` attributi impostati sui `**\*.cshtml` modelli glob e `**\*.razor` .</span><span class="sxs-lookup"><span data-stu-id="4e478-114">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="4e478-115">I file corrispondenti vengono pubblicati.</span><span class="sxs-lookup"><span data-stu-id="4e478-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="4e478-116">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4e478-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="4e478-117">Usare Razor SDK</span><span class="sxs-lookup"><span data-stu-id="4e478-117">Use the Razor SDK</span></span>

<span data-ttu-id="4e478-118">La maggior parte delle App web non è necessario fare riferimento esplicitamente al SDK di Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
<span data-ttu-id="4e478-119">Per usare il Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4e478-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="4e478-120">Usare `Microsoft.NET.Sdk.Razor` anziché `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="4e478-120">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="4e478-121">In genere, un riferimento al pacchetto `Microsoft.AspNetCore.Mvc` è obbligatoria per ricevere le dipendenze aggiuntive necessarie per generare e compilare le pagine Razor e le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-121">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="4e478-122">Come minimo, il progetto deve aggiungere i riferimenti ai pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="4e478-122">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="4e478-123">Il `Microsoft.AspNetCore.Razor.Design` pacchetto fornisce l'attività di compilazione Razor e delle destinazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="4e478-123">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="4e478-124">I pacchetti precedenti sono inclusi in `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="4e478-124">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="4e478-125">Il markup seguente illustra un file di progetto che usa il SDK di Razor per generare i file Razor per un'app ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4e478-125">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="4e478-126">Per usare Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages è consigliabile iniziare con il modello di progetto libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-126">To use the Razor SDK to build class libraries containing Razor views or Razor Pages we recommend starting with the Razor Class Library project template.</span></span> <span data-ttu-id="4e478-127">Una libreria di classi Razor utilizzata per compilare file blazer (Razor) richiede almeno un riferimento al `Microsoft.AspNetCore.Components` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4e478-127">A Razor class library that is used to build Blazor (.razor) files will at minimum require a reference to the `Microsoft.AspNetCore.Components` package.</span></span> <span data-ttu-id="4e478-128">Una libreria di classi Razor usata per creare visualizzazioni o pagine Razor (file con estensione cshtml) richiederà la destinazione `netcoreapp3.0` o una `FrameworkReference` versione più recente e avrà `Microsoft.AspNetCore.App`a.</span><span class="sxs-lookup"><span data-stu-id="4e478-128">A Razor class library that is used to build Razor views or pages (.cshtml files), will require targeting `netcoreapp3.0` or newer and have a `FrameworkReference` to `Microsoft.AspNetCore.App`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="4e478-129">Il `Microsoft.AspNetCore.Razor.Design` e `Microsoft.AspNetCore.Mvc.Razor.Extensions` i pacchetti sono inclusi nel [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4e478-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4e478-130">Tuttavia, la versione l'uzo `Microsoft.AspNetCore.App` riferimento al pacchetto fornisce un metapacchetto all'app che non include la versione più recente di `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="4e478-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="4e478-131">I progetti devono fare riferimento a una versione coerente del `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) in modo che siano incluse le correzioni più recenti in fase di compilazione per Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="4e478-132">Per altre informazioni, vedere [questo problema su GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="4e478-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="4e478-133">Proprietà</span><span class="sxs-lookup"><span data-stu-id="4e478-133">Properties</span></span>

<span data-ttu-id="4e478-134">Le proprietà seguenti controllano il comportamento del Razor SDK come parte della compilazione del progetto:</span><span class="sxs-lookup"><span data-stu-id="4e478-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="4e478-135">`RazorCompileOnBuild` &ndash; Quando `true`, compila e crea l'assembly di Razor come parte della compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="4e478-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="4e478-136">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="4e478-136">Defaults to `true`.</span></span>
* <span data-ttu-id="4e478-137">`RazorCompileOnPublish` &ndash; Quando `true`, compila e crea l'assembly di Razor come parte della pubblicazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="4e478-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="4e478-138">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="4e478-138">Defaults to `true`.</span></span>

<span data-ttu-id="4e478-139">Le proprietà e gli elementi nella tabella seguente vengono utilizzati per configurare input e output per il SDK di Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"
> [!WARNING]
<span data-ttu-id="4e478-140">A partire da ASP.NET Core 3,0, le visualizzazioni MVC o Razor Pages non verranno gestite per impostazione `RazorCompileOnBuild` predefinita `RazorCompileOnPublish` se o è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="4e478-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages will not be served by default if `RazorCompileOnBuild` or `RazorCompileOnPublish` is disabled.</span></span> <span data-ttu-id="4e478-141">Le applicazioni devono aggiungere un riferimento esplicito al `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` pacchetto per aggiungere il supporto per la compilazione di runtime se si basano sulla compilazione di runtime per elaborare i file cshtml.</span><span class="sxs-lookup"><span data-stu-id="4e478-141">Applications must add an explicit reference to `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package to add support for runtime compilation if they rely on runtime compilation to process cshtml files.</span></span>
::: moniker-end


| <span data-ttu-id="4e478-142">Elementi</span><span class="sxs-lookup"><span data-stu-id="4e478-142">Items</span></span> | <span data-ttu-id="4e478-143">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="4e478-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="4e478-144">Elementi Item (file con*estensione cshtml* ) che sono input per la generazione di codice.</span><span class="sxs-lookup"><span data-stu-id="4e478-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="4e478-145">Elementi elemento (file*Razor* ) che sono input per la generazione di codice componente.</span><span class="sxs-lookup"><span data-stu-id="4e478-145">Item elements (*.razor* files) that are inputs to Component code generation.</span></span>
| `RazorCompile` | <span data-ttu-id="4e478-146">Elementi Item (file con*estensione cs* ) che sono input per le destinazioni di compilazione Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="4e478-147">Usare questa `ItemGroup` impostazione per specificare i file aggiuntivi da compilare nell'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="4e478-148">Elementi della voce usati per attributi di generazione del codice per l'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="4e478-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4e478-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="4e478-150">Elementi aggiunti come risorse incorporate nell'assembly generato Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="4e478-151">Proprietà</span><span class="sxs-lookup"><span data-stu-id="4e478-151">Property</span></span> | <span data-ttu-id="4e478-152">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4e478-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="4e478-153">Nome di file (senza estensione) dell'assembly generato da Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="4e478-154">Directory di output di Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="4e478-155">Usato per determinare il set di strumenti utilizzato per compilare l'assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="4e478-156">I valori validi sono `Implicit`, `RazorSDK` e `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="4e478-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="4e478-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="4e478-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="4e478-158">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="4e478-158">Default is `true`.</span></span> <span data-ttu-id="4e478-159">Quando `true`, include i file *Web. config*, *. JSON*e *. cshtml* come contenuto nel progetto.</span><span class="sxs-lookup"><span data-stu-id="4e478-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="4e478-160">Quando viene fatto riferimento tramite `Microsoft.NET.Sdk.Web`, i file sotto *wwwroot* e sono inclusi anche i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="4e478-161">Quando è `true`, include i file *cshtml* degli elementi di `Content` negli elementi di `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="4e478-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="4e478-162">Quando `true`, genera una *. cs* file che contiene gli attributi specificati da `RazorAssemblyAttribute` e include il file nell'output di compilazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="4e478-163">Quando è `true`, aggiunge un set predefinito di attributi degli assembly a `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4e478-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="4e478-164">Quando `true`, le copie `RazorGenerate` gli elementi (*cshtml*) file alla directory di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="4e478-165">In genere, i file Razor non sono necessari per un'app pubblicata se partecipano alla compilazione in fase di compilazione o in fase di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="4e478-166">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="4e478-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="4e478-167">Quando è `true`, copiare gli elementi dell'assembly di riferimento nella directory di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="4e478-168">In genere, gli assembly di riferimento non sono necessari per un'app pubblicata se compilazione Razor si verifica in fase di compilazione o in fase di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="4e478-169">Impostare su `true` se l'app pubblicata richiede la compilazione di runtime.</span><span class="sxs-lookup"><span data-stu-id="4e478-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="4e478-170">Ad esempio, impostare il valore su `true` se l'app modifica *. cshtml* file in fase di esecuzione o utilizza visualizzazioni incorporate.</span><span class="sxs-lookup"><span data-stu-id="4e478-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="4e478-171">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="4e478-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="4e478-172">Quando `true`, tutti gli elementi di contenuto di Razor (*cshtml* file) sono contrassegnati per l'inclusione nel pacchetto NuGet generato.</span><span class="sxs-lookup"><span data-stu-id="4e478-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="4e478-173">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="4e478-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="4e478-174">Quando è `true`, aggiunge gli elementi (*cshtml*) di RazorGenerate come file incorporati nell'assembly Razor generato.</span><span class="sxs-lookup"><span data-stu-id="4e478-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="4e478-175">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="4e478-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="4e478-176">Quando è `true`, usa un processo del server di compilazione permanente per ripartire il lavoro di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="4e478-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="4e478-177">Valore predefinito è il valore di `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="4e478-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="4e478-178">Quando `true`, l'SDK genera attributi aggiuntivi usati da MVC in fase di esecuzione per eseguire l'individuazione delle parti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="4e478-179">Per altre informazioni sulle proprietà, vedere [Proprietà di MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="4e478-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="4e478-180">Destinazioni</span><span class="sxs-lookup"><span data-stu-id="4e478-180">Targets</span></span>

<span data-ttu-id="4e478-181">Il Razor SDK definisce due obiettivi principali:</span><span class="sxs-lookup"><span data-stu-id="4e478-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="4e478-182">`RazorGenerate` &ndash; Genera codice *cs* dei file dalla `RazorGenerate` elementi item.</span><span class="sxs-lookup"><span data-stu-id="4e478-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="4e478-183">Usa la proprietà `RazorGenerateDependsOn` per specificare le altre destinazioni che possono essere eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-183">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="4e478-184">`RazorCompile` &ndash; Le compilazioni generate *cs* i file un assembly di Razor.</span><span class="sxs-lookup"><span data-stu-id="4e478-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="4e478-185">Usare `RazorCompileDependsOn` per specificare le altre destinazioni che possono eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-185">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="4e478-186">`RazorComponentGenerate`Il codice genera file con *estensione cs* per `RazorComponent` gli elementi Item. &ndash;</span><span class="sxs-lookup"><span data-stu-id="4e478-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="4e478-187">Usa la proprietà `RazorComponentGenerateDependsOn` per specificare le altre destinazioni che possono essere eseguite prima o dopo questa destinazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-187">Use `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="4e478-188">Compilazione runtime di visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="4e478-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="4e478-189">Per impostazione predefinita, Razor SDK non pubblica gli assembly di riferimento necessari per eseguire la compilazione runtime.</span><span class="sxs-lookup"><span data-stu-id="4e478-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="4e478-190">Vengono pertanto generati errori di compilazione nel caso in cui il modello di applicazione si basi su una compilazione runtime&mdash;ad esempio, l'app usa visualizzazioni incorporate o modifica le visualizzazioni dopo aver pubblicato l'app.</span><span class="sxs-lookup"><span data-stu-id="4e478-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="4e478-191">Impostare `CopyRefAssembliesToPublishDirectory` su `true` per continuare con la pubblicazione degli assembly di riferimento.</span><span class="sxs-lookup"><span data-stu-id="4e478-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="4e478-192">Per un'app web, assicurarsi che l'app è destinata al `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="4e478-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="4e478-193">Versione della lingua Razor</span><span class="sxs-lookup"><span data-stu-id="4e478-193">Razor language version</span></span>

<span data-ttu-id="4e478-194">Quando la destinazione `Microsoft.NET.Sdk.Web` è SDK, la versione della lingua Razor viene dedotta dalla versione del Framework di destinazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4e478-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="4e478-195">Per i progetti destinati all' `Microsoft.NET.Sdk.Razor` SDK o nel raro caso in cui l'app richieda una versione diversa della lingua Razor rispetto al valore derivato, è possibile configurare una versione impostando la `<RazorLangVersion>` proprietà nel file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="4e478-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="4e478-196">La versione del linguaggio Razor è strettamente integrata con la versione del runtime per cui è stata creata.</span><span class="sxs-lookup"><span data-stu-id="4e478-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="4e478-197">La destinazione di una versione di linguaggio non progettata per il runtime non è supportata e probabilmente genera errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="4e478-197">Targeting a language version that is not designed for the runtime is unsupported, and will likely produce build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e478-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4e478-198">Additional resources</span></span>

* [<span data-ttu-id="4e478-199">Aggiunte al formato csproj per .NET Core</span><span class="sxs-lookup"><span data-stu-id="4e478-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="4e478-200">Elementi di progetto MSBuild comuni</span><span class="sxs-lookup"><span data-stu-id="4e478-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
