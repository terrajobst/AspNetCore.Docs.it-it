---
title: La precompilazione e la compilazione di visualizzazione razor
author: rick-anderson
description: Un documento di riferimento che descrivono come abilitare la compilazione di visualizzazione MVC Razor e precompilazione nelle applicazioni ASP.NET Core.
keywords: ASP.NET Core, la compilazione di visualizzazione Razor, Razor pre-compilazione, la precompilazione Razor
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 6839892c104673af0fd0fd074d368f3f42259d76
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="dc572-104">Compilazione di visualizzazione Razor e precompilazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc572-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="dc572-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc572-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc572-106">Visualizzazioni Razor vengono compilate in fase di esecuzione quando viene richiamata la vista.</span><span class="sxs-lookup"><span data-stu-id="dc572-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="dc572-107">ASP.NET Core 1.1.0 e versione successiva possono facoltativamente compilare visualizzazioni Razor e distribuirli con l'app&mdash;un processo noto come la precompilazione.</span><span class="sxs-lookup"><span data-stu-id="dc572-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="dc572-108">Per impostazione predefinita, i modelli di progetto ASP.NET Core 2. x abilitare precompilazione.</span><span class="sxs-lookup"><span data-stu-id="dc572-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc572-109">Precompilazione di visualizzazione Razor non è attualmente disponibile quando si esegue un [distribuzione indipendente (dimensione a modifica lenta)](/dotnet/core/deploying/#self-contained-deployments-scd) Core ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="dc572-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="dc572-110">La funzionalità sarà disponibile per dimensioni a modifica lenta quando rilascia 2.1.</span><span class="sxs-lookup"><span data-stu-id="dc572-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="dc572-111">Per ulteriori informazioni, vedere [la compilazione di visualizzazione non riesce durante la compilazione incrociata per Linux in Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="dc572-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="dc572-112">Considerazioni sulla precompilazione:</span><span class="sxs-lookup"><span data-stu-id="dc572-112">Precompilation considerations:</span></span>

* <span data-ttu-id="dc572-113">La precompilazione di visualizzazioni comporta un più piccolo pubblicato bundle e tempi di avvio.</span><span class="sxs-lookup"><span data-stu-id="dc572-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="dc572-114">È possibile modificare i file Razor dopo la precompilazione di viste.</span><span class="sxs-lookup"><span data-stu-id="dc572-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="dc572-115">Le viste modificate non saranno presenti nel bundle pubblicato.</span><span class="sxs-lookup"><span data-stu-id="dc572-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="dc572-116">Per distribuire precompilate viste:</span><span class="sxs-lookup"><span data-stu-id="dc572-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc572-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc572-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dc572-118">Se il progetto è destinato a .NET Framework, includere un riferimento pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="dc572-118">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="dc572-119">Se il progetto è destinato a .NET Core, non sono necessarie modifiche.</span><span class="sxs-lookup"><span data-stu-id="dc572-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="dc572-120">I modelli di progetto ASP.NET Core 2. x viene impostata in modo implicito `MvcRazorCompileOnPublish` a `true` per impostazione predefinita, ovvero il nodo può essere rimosso in modo sicuro dal *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="dc572-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="dc572-121">Se si preferisce, è necessario essere espliciti sono non crea alcun problema nell'impostazione di `MvcRazorCompileOnPublish` proprietà `true`.</span><span class="sxs-lookup"><span data-stu-id="dc572-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="dc572-122">Nell'esempio *csproj* ed evidenzia questa impostazione:</span><span class="sxs-lookup"><span data-stu-id="dc572-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc572-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc572-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dc572-124">Impostare `MvcRazorCompileOnPublish` a `true`e includere un riferimento pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="dc572-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="dc572-125">Nell'esempio *csproj* ed evidenzia queste impostazioni:</span><span class="sxs-lookup"><span data-stu-id="dc572-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="dc572-126">Preparare l'app per un [distribuzione dipendenti dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) eseguendo un comando simile al seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="dc572-126">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="dc572-127">Oggetto *< Nome_Progetto >. PrecompiledViews.dll* file contenente le visualizzazioni Razor compilate, viene generato quando la precompilazione ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="dc572-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="dc572-128">Ad esempio, la schermata seguente illustra il contenuto di *cshtml* all'interno di *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="dc572-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Visualizzazioni Razor all'interno di DLL](view-compilation/_static/razor-views-in-dll.png)
