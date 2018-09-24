---
title: Compilazione e precompilazione dei file Razor in ASP.NET Core
author: rick-anderson
description: Informazioni sui vantaggi della precompilazione dei file Razor e su come eseguire la precompilazione dei file Razor in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: f5888cf43d8d8192acedaa33b3fa0f313737fc9b
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011287"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="cfa53-103">Compilazione del file Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfa53-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="cfa53-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cfa53-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="cfa53-105">Un file Razor viene compilato in fase di runtime, quando viene richiamata la visualizzazione MVC associata.</span><span class="sxs-lookup"><span data-stu-id="cfa53-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="cfa53-106">La pubblicazione dei file Razor in fase di compilazione non è supportata.</span><span class="sxs-lookup"><span data-stu-id="cfa53-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="cfa53-107">I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.</span><span class="sxs-lookup"><span data-stu-id="cfa53-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cfa53-108">Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata.</span><span class="sxs-lookup"><span data-stu-id="cfa53-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="cfa53-109">La pubblicazione dei file Razor in fase di compilazione non è supportata.</span><span class="sxs-lookup"><span data-stu-id="cfa53-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="cfa53-110">I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.</span><span class="sxs-lookup"><span data-stu-id="cfa53-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cfa53-111">Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata.</span><span class="sxs-lookup"><span data-stu-id="cfa53-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="cfa53-112">I file Razor vengono compilati sia in fase di compilazione che in fase di pubblicazione tramite [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="cfa53-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="cfa53-113">Considerazioni sulla precompilazione</span><span class="sxs-lookup"><span data-stu-id="cfa53-113">Precompilation considerations</span></span>

<span data-ttu-id="cfa53-114">Ecco gli effetti collaterali della precompilazione dei file Razor:</span><span class="sxs-lookup"><span data-stu-id="cfa53-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="cfa53-115">Dimensioni inferiori del bundle pubblicato</span><span class="sxs-lookup"><span data-stu-id="cfa53-115">A smaller published bundle</span></span>
* <span data-ttu-id="cfa53-116">Minore tempo di avvio</span><span class="sxs-lookup"><span data-stu-id="cfa53-116">A faster startup time</span></span>
* <span data-ttu-id="cfa53-117">Non è possibile modificare i file Razor&mdash;il contenuto associato è assente dal bundle pubblicato.</span><span class="sxs-lookup"><span data-stu-id="cfa53-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="cfa53-118">Distribuire file precompilati</span><span class="sxs-lookup"><span data-stu-id="cfa53-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cfa53-119">La compilazione di file Razor in fase di build e pubblicazione è abilitata per impostazione predefinita da Razor SDK.</span><span class="sxs-lookup"><span data-stu-id="cfa53-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="cfa53-120">La modifica dei file Razor dopo che sono stati aggiornati è supportata in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="cfa53-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="cfa53-121">Per impostazione predefinita, con l'app viene distribuito solo il file *Views.dll* compilato. Non viene distribuito alcun file con estensione *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cfa53-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfa53-122">Lo strumento di precompilazione verrà rimosso in ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="cfa53-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="cfa53-123">È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="cfa53-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="cfa53-124">Razor SDK è attivo solo se nel file di progetto non sono impostate proprietà specifiche di precompilazione.</span><span class="sxs-lookup"><span data-stu-id="cfa53-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="cfa53-125">Se ad esempio si imposta la proprietà `MvcRazorCompileOnPublish` del file con estensione *csproj* su `true`, Razor SDK viene disabilitato.</span><span class="sxs-lookup"><span data-stu-id="cfa53-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cfa53-126">Se il progetto è destinato a .NET Framework, installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="cfa53-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="cfa53-127">Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.</span><span class="sxs-lookup"><span data-stu-id="cfa53-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="cfa53-128">I modelli di progetto ASP.NET Core 2.x impostano in modo implicito la proprietà `MvcRazorCompileOnPublish` su `true` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="cfa53-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="cfa53-129">Di conseguenza, questo elemento può essere rimosso senza problemi dal file con estensione *csproj*.</span><span class="sxs-lookup"><span data-stu-id="cfa53-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfa53-130">Lo strumento di precompilazione verrà rimosso in ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="cfa53-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="cfa53-131">È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="cfa53-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="cfa53-132">La precompilazione dei file Razor non è disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="cfa53-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="cfa53-133">Impostare la proprietà `MvcRazorCompileOnPublish` su `true`e installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="cfa53-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="cfa53-134">Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:</span><span class="sxs-lookup"><span data-stu-id="cfa53-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="cfa53-135">Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="cfa53-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="cfa53-136">Ad esempio, eseguire il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="cfa53-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="cfa53-137">Al termine della compilazione, viene prodotto un file *<nome_progetto>.PrecompiledViews.dll* contenente i file Razor compilati.</span><span class="sxs-lookup"><span data-stu-id="cfa53-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="cfa53-138">Lo screenshot seguente, ad esempio, illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="cfa53-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cfa53-140">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cfa53-140">Additional resources</span></span>

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
