---
title: Migrazione da ASP.NET Core 1.x a 2.0
author: scottaddie
description: "In questo articolo vengono illustrati i prerequisiti e i passaggi più comuni per la migrazione di un progetto di ASP.NET Core 1.x su ASP.NET 2.0 Core."
keywords: ASP.NET Core, eseguire la migrazione
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: c14e7d61e8b353c18fc4a4f2bf3658069982bad5
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="71f78-104">Migrazione da ASP.NET Core 1.x a 2.0</span><span class="sxs-lookup"><span data-stu-id="71f78-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="71f78-105">Di [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="71f78-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="71f78-106">In questo articolo si procederà con l'aggiornamento di un progetto ASP.NET Core 1.x esistente ad ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="71f78-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="71f78-107">La migrazione dell'applicazione ad ASP.NET 2.0 Core consente di sfruttare le [molte nuove caratteristiche e i miglioramenti delle prestazioni](https://go.microsoft.com/fwlink/?linkid=854094).</span><span class="sxs-lookup"><span data-stu-id="71f78-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://go.microsoft.com/fwlink/?linkid=854094).</span></span> 

<span data-ttu-id="71f78-108">Le applicazioni esistenti di ASP.NET Core 1.x si basano su modelli di progetto specifico della versione.</span><span class="sxs-lookup"><span data-stu-id="71f78-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="71f78-109">Quando il framework di ASP.NET Core si evolve, i modelli del progetto fanno la stessa cosa, così come il codice di avvio in essi contenuti.</span><span class="sxs-lookup"><span data-stu-id="71f78-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="71f78-110">Oltre ad aggiornare il framework di ASP.NET Core, è necessario aggiornare il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="71f78-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="71f78-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="71f78-111">Prerequisites</span></span>
<span data-ttu-id="71f78-112">Vedere [Introduzione ad ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="71f78-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="71f78-113">Aggiornare Moniker della versione di .NET Framework di destinazione (TFM, Target Framework Moniker)</span><span class="sxs-lookup"><span data-stu-id="71f78-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="71f78-114">I progetti destinati a .NET Core devono usare il [TFM](/dotnet/standard/frameworks#referring-to-frameworks) di una versione successiva o uguale a .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="71f78-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="71f78-115">Cercare il nodo `<TargetFramework>` nel file *csproj*, e sostituire il testo interno con `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="71f78-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

<span data-ttu-id="71f78-116">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span><span class="sxs-lookup"><span data-stu-id="71f78-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span></span>

<span data-ttu-id="71f78-117">I progetti destinati a .NET Framework devono usare il TFM di una versione successiva o uguale a .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="71f78-117">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="71f78-118">Cercare il nodo `<TargetFramework>` nel file *csproj*, e sostituire il testo interno con `net461`:</span><span class="sxs-lookup"><span data-stu-id="71f78-118">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

<span data-ttu-id="71f78-119">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span><span class="sxs-lookup"><span data-stu-id="71f78-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span></span>

> [!NOTE]
> <span data-ttu-id="71f78-120">.NET core 2.0 offre una quantità di superficie maggiore rispetto a .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="71f78-120">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="71f78-121">Se il destinatario è .NET Framework esclusivamente a causa della mancanza di API in .NET Core 1.x, avere .NET Core 2.0 come destinatario potrebbe funzionare.</span><span class="sxs-lookup"><span data-stu-id="71f78-121">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="71f78-122">Aggiornare la versione di .NET Core SDK in global.json</span><span class="sxs-lookup"><span data-stu-id="71f78-122">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="71f78-123">Se la soluzione si basa su un file [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) per avere come destinatario una specifica versione di .NET Core SDK, aggiornare la relativa proprietà `version` per usare la versione 2.0 installata nel computer:</span><span class="sxs-lookup"><span data-stu-id="71f78-123">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

<span data-ttu-id="71f78-124">[!code-json[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="71f78-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span></span>

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="71f78-125">Aggiornare i riferimenti del pacchetto</span><span class="sxs-lookup"><span data-stu-id="71f78-125">Update package references</span></span>
<span data-ttu-id="71f78-126">Il file *csproj* in un progetto di 1.x elenca ogni pacchetto NuGet usato dal progetto.</span><span class="sxs-lookup"><span data-stu-id="71f78-126">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="71f78-127">In un progetto ASP.NET Core 2.0 destinato a .NET Core 2.0, un singolo riferimento del [metapacchetto](xref:fundamentals/metapackage) nel file *csproj* sostituisce la raccolta di pacchetti:</span><span class="sxs-lookup"><span data-stu-id="71f78-127">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

<span data-ttu-id="71f78-128">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span><span class="sxs-lookup"><span data-stu-id="71f78-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span></span>

<span data-ttu-id="71f78-129">Tutte le funzionalità di ASP.NET Core 2.0 e di Entity Framework Core 2.0 sono incluse nel metapacchetto.</span><span class="sxs-lookup"><span data-stu-id="71f78-129">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="71f78-130">I progetti di ASP.NET Core 2.0 con destinazione .NET Framework devono fare riferimento a singoli pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="71f78-130">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="71f78-131">Aggiornamento dell'attributo `Version` di ogni `<PackageReference />` nodo a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="71f78-131">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="71f78-132">Ad esempio, ecco l'elenco dei `<PackageReference />` nodi usati in un progetto ASP.NET Core 2.0 tipico destinato a .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="71f78-132">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

<span data-ttu-id="71f78-133">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span><span class="sxs-lookup"><span data-stu-id="71f78-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span></span>

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="71f78-134">Strumenti dell'interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="71f78-134">Update .NET Core CLI tools</span></span>
<span data-ttu-id="71f78-135">Nel file *.csproj* aggiornare l'attributo `Version` di ogni nodo `<DotNetCliToolReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="71f78-135">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="71f78-136">Ad esempio, ecco l'elenco degli strumenti dell'interfaccia della riga di comando usati in un progetto ASP.NET Core 2.0 tipico destinato a .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="71f78-136">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

<span data-ttu-id="71f78-137">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="71f78-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span></span>

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="71f78-138">Rinominare la proprietà di Fallback di destinazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="71f78-138">Rename Package Target Fallback property</span></span>
<span data-ttu-id="71f78-139">Il file *csproj* di un progetto 1.x ha usato un nodo `PackageTargetFallback` e una variabile:</span><span class="sxs-lookup"><span data-stu-id="71f78-139">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

<span data-ttu-id="71f78-140">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="71f78-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span></span>

<span data-ttu-id="71f78-141">Rinominare il nodo e la variabile a `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="71f78-141">Rename both the node and variable to `AssetTargetFallback`:</span></span>

<span data-ttu-id="71f78-142">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="71f78-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span></span>

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="71f78-143">Aggiornare il metodo Main in Program.cs</span><span class="sxs-lookup"><span data-stu-id="71f78-143">Update Main method in Program.cs</span></span>
<span data-ttu-id="71f78-144">Nei progetti di 1.x, il metodo `Main` di *Program.cs* era simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="71f78-144">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

<span data-ttu-id="71f78-145">[!code-csharp[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span><span class="sxs-lookup"><span data-stu-id="71f78-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span></span>

<span data-ttu-id="71f78-146">Nei progetti 2.0, il metodo `Main` di *Program.cs* è stato semplificato:</span><span class="sxs-lookup"><span data-stu-id="71f78-146">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

<span data-ttu-id="71f78-147">[!code-csharp[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="71f78-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span></span>

<span data-ttu-id="71f78-148">L'adozione di questo nuovo modello 2.0 è altamente consigliabile ed è necessario affinché le funzionalità del prodotto come [Migrazioni di Entity Framework](xref:data/ef-mvc/migrations) funzionino.</span><span class="sxs-lookup"><span data-stu-id="71f78-148">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="71f78-149">Ad esempio, l'esecuzione di `Update-Database` dalla finestra della Console di Gestione pacchetti o di `dotnet ef database update` dalla riga di comando (nei progetti convertiti in ASP.NET Core 2.0) genera l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="71f78-149">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="71f78-150">Revisionare le impostazioni di compilazione della vista Razor</span><span class="sxs-lookup"><span data-stu-id="71f78-150">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="71f78-151">Tempi di avvio dell'applicazione più rapidi e aggregazioni pubblicate più piccole sono di importanza fondamentale per l'utente.</span><span class="sxs-lookup"><span data-stu-id="71f78-151">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="71f78-152">Per questi motivi, [la compilazione della vista Razor](xref:mvc/views/view-compilation) è abilitata per impostazione predefinita in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="71f78-152">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="71f78-153">L'impostazione della proprietà `MvcRazorCompileOnPublish` su true non è più necessaria.</span><span class="sxs-lookup"><span data-stu-id="71f78-153">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="71f78-154">A meno che non si stia disattivando la compilazione della vista, la proprietà può essere rimossa dal file *csproj*.</span><span class="sxs-lookup"><span data-stu-id="71f78-154">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="71f78-155">Quando la destinazione è .NET Framework, è necessario fare comunque riferimento in modo esplicito al pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) nel file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="71f78-155">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

<span data-ttu-id="71f78-156">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span><span class="sxs-lookup"><span data-stu-id="71f78-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span></span>

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="71f78-157">Basarsi sulle funzionalità di Application Insights "Light-Up"</span><span class="sxs-lookup"><span data-stu-id="71f78-157">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="71f78-158">Un semplice programma di installazione di strumentazione delle prestazioni dell'applicazione è importante.</span><span class="sxs-lookup"><span data-stu-id="71f78-158">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="71f78-159">È ora possibile basarsi sulle nuove funzionalità "light-up" di [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponibili negli strumenti di Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="71f78-159">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="71f78-160">Per impostazione predefinita, i progetti ASP.NET Core 1.1 creati in Visual Studio 2017 hanno aggiunto Application Insights.</span><span class="sxs-lookup"><span data-stu-id="71f78-160">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="71f78-161">Se non si usa Application Insights SDK direttamente, di fuori di *Program.cs* e *Startup.cs*, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="71f78-161">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="71f78-162">Rimuovere il nodo `<PackageReference />` seguente dal file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="71f78-162">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    <span data-ttu-id="71f78-163">[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span><span class="sxs-lookup"><span data-stu-id="71f78-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span></span>

2. <span data-ttu-id="71f78-164">Rimuovere la chiamata del metodo di estensione `UseApplicationInsights` da *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="71f78-164">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    <span data-ttu-id="71f78-165">[!code-csharp[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="71f78-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span></span>

3. <span data-ttu-id="71f78-166">Rimuovere la chiamata di API sul lato client di Application Insights da *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="71f78-166">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="71f78-167">Comprende le due righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="71f78-167">It comprises the following two lines of code:</span></span>

    <span data-ttu-id="71f78-168">[!code-cshtml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span><span class="sxs-lookup"><span data-stu-id="71f78-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span></span>

<span data-ttu-id="71f78-169">Se si usa Application Insights SDK direttamente, continuare a farlo.</span><span class="sxs-lookup"><span data-stu-id="71f78-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="71f78-170">Il [metapacchetto](xref:fundamentals/metapackage) 2.0 comprende la versione più recente di Application Insights, pertanto viene visualizzato un errore di downgrade del pacchetto se si fa riferimento a una versione precedente.</span><span class="sxs-lookup"><span data-stu-id="71f78-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="71f78-171">Adottare i miglioramenti di Autenticazione/Identità</span><span class="sxs-lookup"><span data-stu-id="71f78-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="71f78-172">ASP.NET Core 2.0 ha un nuovo modello di autenticazione e un numero di modifiche significative per ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="71f78-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="71f78-173">Se il progetto è stato creato con Account utente abilitati o se è stata aggiunta manualmente l'autenticazione o l'identità, vedere [Migrazione di autenticazione e identità ad ASP.NET 2.0 Core](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="71f78-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71f78-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="71f78-174">Additional Resources</span></span>
- [<span data-ttu-id="71f78-175">Modifiche importanti apportate in ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="71f78-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
