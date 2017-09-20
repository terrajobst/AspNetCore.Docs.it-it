---
title: La precompilazione e la compilazione di visualizzazione razor
author: rick-anderson
description: Un documento di riferimento che descrivono come abilitare la compilazione di visualizzazione MVC Razor e precompilazione nelle applicazioni ASP.NET Core.
keywords: ASP.NET Core, la compilazione di visualizzazione Razor, Razor pre-compilazione, la precompilazione Razor
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 0cb61315916d1b38f7cab3339e150c446fb69d98
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="3a88e-104">Compilazione di visualizzazione Razor e precompilazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a88e-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="3a88e-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3a88e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3a88e-106">Visualizzazioni Razor vengono compilate in fase di esecuzione quando viene richiamata la vista.</span><span class="sxs-lookup"><span data-stu-id="3a88e-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="3a88e-107">ASP.NET Core 1.1.0 e versione successiva possono facoltativamente compilare visualizzazioni Razor e distribuirli con l'app &mdash; un processo noto come la precompilazione.</span><span class="sxs-lookup"><span data-stu-id="3a88e-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="3a88e-108">Per impostazione predefinita, i modelli di progetto ASP.NET Core 2. x abilitare precompilazione.</span><span class="sxs-lookup"><span data-stu-id="3a88e-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="3a88e-109">Precompilazione di visualizzazione Razor non è disponibile quando si esegue un [Self-Contained distribuzione](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) nelle versioni di ASP.NET Core 2.0.0 e versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="3a88e-109">Razor view precompilation is unavailable when doing a [Self-Contained Deployment](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core versions 2.0.0 and earlier.</span></span>

<span data-ttu-id="3a88e-110">Considerazioni sulla precompilazione:</span><span class="sxs-lookup"><span data-stu-id="3a88e-110">Precompilation considerations:</span></span>

* <span data-ttu-id="3a88e-111">La precompilazione di visualizzazioni comporta un più piccolo pubblicato bundle e tempi di avvio.</span><span class="sxs-lookup"><span data-stu-id="3a88e-111">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="3a88e-112">È possibile modificare i file Razor dopo la precompilazione di viste.</span><span class="sxs-lookup"><span data-stu-id="3a88e-112">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="3a88e-113">Le viste modificate non saranno presenti nel bundle pubblicato.</span><span class="sxs-lookup"><span data-stu-id="3a88e-113">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="3a88e-114">Per distribuire precompilate viste:</span><span class="sxs-lookup"><span data-stu-id="3a88e-114">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3a88e-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3a88e-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3a88e-116">Se il progetto è destinato a .NET Framework, includere un riferimento pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="3a88e-116">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="3a88e-117">Se il progetto è destinato a .NET Core, non sono necessarie modifiche.</span><span class="sxs-lookup"><span data-stu-id="3a88e-117">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="3a88e-118">I modelli di progetto ASP.NET Core 2. x viene impostata in modo implicito `MvcRazorCompileOnPublish` a `true` per impostazione predefinita, ovvero il nodo può essere rimosso in modo sicuro dal *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="3a88e-118">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="3a88e-119">Se si preferisce, è necessario essere espliciti sono non crea alcun problema nell'impostazione di `MvcRazorCompileOnPublish` proprietà `true`.</span><span class="sxs-lookup"><span data-stu-id="3a88e-119">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="3a88e-120">Nell'esempio *csproj* ed evidenzia questa impostazione:</span><span class="sxs-lookup"><span data-stu-id="3a88e-120">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3a88e-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3a88e-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3a88e-122">Impostare `MvcRazorCompileOnPublish` a `true`e includere un riferimento pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="3a88e-122">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="3a88e-123">Nell'esempio *csproj* ed evidenzia queste impostazioni:</span><span class="sxs-lookup"><span data-stu-id="3a88e-123">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
