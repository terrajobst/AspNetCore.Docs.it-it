---
title: Helper tag di ambiente in ASP.NET Core
author: pkellner
description: Helper tag di ambiente ASP.NET Core definito con tutte le proprietà
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887676"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="59d36-103">Helper tag di ambiente in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59d36-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="59d36-104">Di [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="59d36-104">By [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="59d36-105">L'helper tag di ambiente esegue il rendering condizionale del proprio contenuto in base all'[ambiente host](xref:fundamentals/environments) corrente.</span><span class="sxs-lookup"><span data-stu-id="59d36-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="59d36-106">L'unico attributo dell'helper tag di ambiente, `names`, è un elenco delimitato da virgole di nomi di ambiente.</span><span class="sxs-lookup"><span data-stu-id="59d36-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="59d36-107">Se nessuno dei nomi di ambiente specificato corrisponde all'ambiente corrente, viene eseguito il rendering del contenuto incluso.</span><span class="sxs-lookup"><span data-stu-id="59d36-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="59d36-108">Per una panoramica degli helper tag, vedere <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="59d36-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="59d36-109">Attributi dell'helper tag di ambiente</span><span class="sxs-lookup"><span data-stu-id="59d36-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="59d36-110">nomi</span><span class="sxs-lookup"><span data-stu-id="59d36-110">names</span></span>

<span data-ttu-id="59d36-111">`names` accetta un singolo nome di ambiente host o un elenco delimitato da virgole di nomi di ambiente, che attiva il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="59d36-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="59d36-112">I valori di ambiente vengono confrontati con il valore corrente restituito da [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="59d36-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="59d36-113">Il confronto non applica la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="59d36-113">The comparison ignores case.</span></span>

<span data-ttu-id="59d36-114">L'esempio seguente usa un helper tag di ambiente.</span><span class="sxs-lookup"><span data-stu-id="59d36-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="59d36-115">Il rendering del contenuto viene eseguito se l'ambiente host è Staging o Production:</span><span class="sxs-lookup"><span data-stu-id="59d36-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="59d36-116">Attributi include ed exclude</span><span class="sxs-lookup"><span data-stu-id="59d36-116">include and exclude attributes</span></span>

<span data-ttu-id="59d36-117">Gli attributi `include` ed `exclude` controllano il rendering del contenuto incluso in base ai nomi di ambiente host inclusi o esclusi.</span><span class="sxs-lookup"><span data-stu-id="59d36-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="59d36-118">include</span><span class="sxs-lookup"><span data-stu-id="59d36-118">include</span></span>

<span data-ttu-id="59d36-119">La proprietà `include` ha un comportamento simile all'attributo `names`.</span><span class="sxs-lookup"><span data-stu-id="59d36-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="59d36-120">Un ambiente elencato nel valore dell'attributo `include` deve corrispondere all'ambiente host dell'app ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) per il rendering del contenuto del tag `<environment>`.</span><span class="sxs-lookup"><span data-stu-id="59d36-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="59d36-121">exclude</span><span class="sxs-lookup"><span data-stu-id="59d36-121">exclude</span></span>

<span data-ttu-id="59d36-122">Al contrario dell'attributo `include`, il rendering del contenuto del tag `<environment>` viene eseguito quando l'ambiente host non corrisponde a un ambiente elencato nel valore dell'attributo `exclude`.</span><span class="sxs-lookup"><span data-stu-id="59d36-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="59d36-123">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="59d36-123">Additional resources</span></span>

* <xref:fundamentals/environments>
