---
title: Compilazione del file Razor in ASP.NET Core
author: rick-anderson
description: Informazioni sulla compilazione dei file Razor in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661105"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="1c5c1-103">Compilazione del file Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c5c1-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="1c5c1-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c5c1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="1c5c1-105">Un file Razor viene compilato in fase di runtime, quando viene richiamata la visualizzazione MVC associata.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="1c5c1-106">La pubblicazione dei file Razor in fase di compilazione non è supportata.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="1c5c1-107">I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1c5c1-108">Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="1c5c1-109">La pubblicazione dei file Razor in fase di compilazione non è supportata.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="1c5c1-110">I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="1c5c1-111">Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="1c5c1-112">I file Razor vengono compilati sia in fase di compilazione che in fase di pubblicazione tramite [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c5c1-113">I file Razor con estensione *cshtml* vengono compilati in fase di compilazione e di pubblicazione tramite [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-113">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="1c5c1-114">Facoltativamente, è possibile abilitare la compilazione in fase di runtime configurando l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="1c5c1-115">Compilazione Razor</span><span class="sxs-lookup"><span data-stu-id="1c5c1-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c5c1-116">La compilazione di file Razor in fase di build e pubblicazione è abilitata per impostazione predefinita da Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="1c5c1-117">Quando abilitata, la compilazione in fase di runtime funge da complemento alla compilazione in fase di build, consentendo di aggiornare i file Razor in caso di modifica.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="1c5c1-118">La compilazione di file Razor in fase di build e pubblicazione è abilitata per impostazione predefinita da Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="1c5c1-119">La modifica dei file Razor dopo che sono stati aggiornati è supportata in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="1c5c1-120">Per impostazione predefinita, vengono distribuiti con l'app solo i file *Views.dll* compilati e non i file *cshtml* o gli assembly di riferimento necessari per compilare i file Razor con l'app.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c5c1-121">Lo strumento di precompilazione è stato deprecato e verrà rimosso in ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="1c5c1-122">È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="1c5c1-123">Razor SDK è attivo solo se nel file di progetto non sono impostate proprietà specifiche di precompilazione.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="1c5c1-124">Se ad esempio si imposta la proprietà *del file con estensione*csproj`MvcRazorCompileOnPublish` su `true`, Razor SDK viene disabilitato.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1c5c1-125">Se il progetto è destinato a .NET Framework, installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="1c5c1-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="1c5c1-126">Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="1c5c1-127">I modelli di progetto ASP.NET Core 2.x impostano in modo implicito la proprietà `MvcRazorCompileOnPublish` su `true` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="1c5c1-128">Di conseguenza, questo elemento può essere rimosso senza problemi dal file con estensione *csproj*.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c5c1-129">Lo strumento di precompilazione è stato deprecato e verrà rimosso in ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="1c5c1-130">È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="1c5c1-131">La precompilazione dei file Razor non è disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="1c5c1-132">Impostare la proprietà `MvcRazorCompileOnPublish` su `true`e installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="1c5c1-133">Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="1c5c1-134">Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="1c5c1-135">Ad esempio, eseguire il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-135">For example, execute the following command at the project root:</span></span>

```dotnetcli
dotnet publish -c Release
```

<span data-ttu-id="1c5c1-136">Al termine della compilazione, viene prodotto un file *\<nome_progetto>.PrecompiledViews.dll* contenente i file Razor compilati.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="1c5c1-137">Lo screenshot seguente, ad esempio, illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="1c5c1-139">Compilazione runtime</span><span class="sxs-lookup"><span data-stu-id="1c5c1-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="1c5c1-140">La compilazione in fase di build è integrata dalla compilazione in fase di runtime dei file Razor.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="1c5c1-141">ASP.NET Core MVC ricompilerà i file Razor quando il contenuto di un file *cshtml* cambia.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="1c5c1-142">La compilazione in fase di build è integrata dalla compilazione in fase di runtime dei file Razor.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="1c5c1-143">Il <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Ottiene o imposta un valore che determina se i file Razor (visualizzazioni Razor e Razor Pages) vengono ricompilati e aggiornati in caso di modifica dei file sul disco.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="1c5c1-144">Il valore predefinito è `true` per:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-144">The default value is `true` for:</span></span>

* <span data-ttu-id="1c5c1-145">Se la versione di compatibilità dell'app è impostata su <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="1c5c1-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="1c5c1-146">Se la versione di compatibilità dell'app è impostata su <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> o versioni successive e l'app è nell'ambiente di sviluppo <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="1c5c1-147">In altre parole, i file Razor non vengono ricompilati in un ambiente non di sviluppo a meno che non sia stato impostato in modo esplicito <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-147">In other words, Razor files wouldn't recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="1c5c1-148">Per indicazioni ed esempi di impostazione della versione di compatibilità dell'app, vedere <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c5c1-149">Per abilitare la compilazione in fase di esecuzione per tutti gli ambienti e le modalità di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-149">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="1c5c1-150">Installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).</span><span class="sxs-lookup"><span data-stu-id="1c5c1-150">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="1c5c1-151">Aggiornare il metodo `Startup.ConfigureServices` del progetto in modo da includere una chiamata a `AddRazorRuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-151">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="1c5c1-152">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-152">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="1c5c1-153">Abilitare in modo condizionale la compilazione del runtime</span><span class="sxs-lookup"><span data-stu-id="1c5c1-153">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="1c5c1-154">È possibile abilitare la compilazione di runtime in modo che sia disponibile solo per lo sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-154">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="1c5c1-155">L'abilitazione condizionale in questo modo garantisce che l'output pubblicato:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-155">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="1c5c1-156">USA viste compilate.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-156">Uses compiled views.</span></span>
* <span data-ttu-id="1c5c1-157">Dimensioni minori.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-157">Is smaller in size.</span></span>
* <span data-ttu-id="1c5c1-158">Non Abilita i controlli file nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-158">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="1c5c1-159">Per abilitare la compilazione in fase di esecuzione in base all'ambiente e alla modalità di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-159">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="1c5c1-160">Fare riferimento in modo condizionale al pacchetto [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) in base al valore di `Configuration` attivo:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-160">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="1c5c1-161">Aggiornare il metodo `Startup.ConfigureServices` del progetto in modo da includere una chiamata a `AddRazorRuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="1c5c1-161">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="1c5c1-162">Eseguire in modo condizionale `AddRazorRuntimeCompilation` in modo che venga eseguito in modalità di debug solo quando la variabile `ASPNETCORE_ENVIRONMENT` è impostata su `Development`:</span><span class="sxs-lookup"><span data-stu-id="1c5c1-162">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1c5c1-163">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1c5c1-163">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
