---
title: Compilazione e precompilazione delle visualizzazioni Razor
author: rick-anderson
description: Documento di riferimento che descrive come abilitare la compilazione e la precompilazione Razor MVC in applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="9d71a-103">Compilazione e precompilazione delle visualizzazioni Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d71a-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="9d71a-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9d71a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9d71a-105">Le visualizzazioni Razor vengono compilate in fase di esecuzione quando la viene richiamata la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9d71a-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="9d71a-106">ASP.NET Core 1.1.0 e versioni successive possono facoltativamente compilare e distribuire le visualizzazioni Razor usando l'app &mdash;. Questo processo è noto come precompilazione.</span><span class="sxs-lookup"><span data-stu-id="9d71a-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="9d71a-107">Per impostazione predefinita, la precompilazione è abilitata nei modelli di progetto ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="9d71a-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9d71a-108">La precompilazione delle visualizzazioni Razor non è attualmente disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9d71a-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9d71a-109">La funzionalità sarà disponibile per questo tipo di distribuzione a partire dalla versione 2.1.</span><span class="sxs-lookup"><span data-stu-id="9d71a-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="9d71a-110">Per altre informazioni, vedere [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Errore di compilazione delle visualizzazioni durante la cross-compilazione per Linux in Windows.</span><span class="sxs-lookup"><span data-stu-id="9d71a-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="9d71a-111">Considerazioni sulla precompilazione:</span><span class="sxs-lookup"><span data-stu-id="9d71a-111">Precompilation considerations:</span></span>

* <span data-ttu-id="9d71a-112">La precompilazione delle visualizzazioni consente di ridurre le dimensioni del bundle pubblicato e velocizzare il tempo di avvio.</span><span class="sxs-lookup"><span data-stu-id="9d71a-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="9d71a-113">Non è possibile modificare i file Razor dopo aver precompliato le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="9d71a-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="9d71a-114">Le visualizzazioni modificate non saranno contenute nel bundle pubblicato.</span><span class="sxs-lookup"><span data-stu-id="9d71a-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="9d71a-115">Per distribuire le visualizzazioni precompilate:</span><span class="sxs-lookup"><span data-stu-id="9d71a-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9d71a-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9d71a-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9d71a-117">Se .NET Framework è la destinazione del progetto, includere un riferimento al pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="9d71a-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="9d71a-118">Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.</span><span class="sxs-lookup"><span data-stu-id="9d71a-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="9d71a-119">Per impostazione predefinita, i modelli di progetto ASP.NET Core 2.x impostano in modo implicito `MvcRazorCompileOnPublish` su `true`. In questo modo il nodo può essere rimosso in modo sicuro dal file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9d71a-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="9d71a-120">Se si preferisce la modalità esplicita, è possibile impostare la proprietà `MvcRazorCompileOnPublish` su `true`.</span><span class="sxs-lookup"><span data-stu-id="9d71a-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="9d71a-121">Nell'esempio *.csproj* seguente viene evidenziata questa impostazione:</span><span class="sxs-lookup"><span data-stu-id="9d71a-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9d71a-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9d71a-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9d71a-123">Impostare `MvcRazorCompileOnPublish` su `true`e includere un riferimento al pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="9d71a-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="9d71a-124">Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:</span><span class="sxs-lookup"><span data-stu-id="9d71a-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="9d71a-125">Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) eseguendo un comando simile al seguente alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="9d71a-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="9d71a-126">Quando la precompilazione viene completata, si genera un file *<nome_progetto>.PrecompiledViews.dll* contenente le visualizzazioni Razor compilate.</span><span class="sxs-lookup"><span data-stu-id="9d71a-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="9d71a-127">Ad esempio, lo screenshot seguente illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="9d71a-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)
