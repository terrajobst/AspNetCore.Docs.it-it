---
title: Helper tag di script in ASP.NET Core
author: rick-anderson
ms.author: riande
description: Individuare gli attributi degli helper tag di script ASP.NET Core e il ruolo di ciascun attributo per estendere il comportamento del tag di script HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: c3d9148bd62dcc045873cc3a72884ae458349d70
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317114"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="24818-103">Helper tag di script in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24818-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="24818-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="24818-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="24818-105">L' [Helper tag script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) genera un collegamento a un file di script primario o di fallback.</span><span class="sxs-lookup"><span data-stu-id="24818-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="24818-106">In genere il file di script primario si trova in una rete per la [distribuzione di contenuti](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="24818-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="24818-107">L'helper tag script consente di specificare una rete CDN per il file di script e un fallback quando la rete CDN non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="24818-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="24818-108">L'helper tag script offre il vantaggio in merito alle prestazioni di una rete CDN con l'affidabilità dell'hosting locale.</span><span class="sxs-lookup"><span data-stu-id="24818-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="24818-109">Il markup Razor seguente mostra un elemento `script` con un fallback:</span><span class="sxs-lookup"><span data-stu-id="24818-109">The following Razor markup shows a `script` element with a fallback:</span></span>

```HTML
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="24818-110">Attributi di helper tag di script di uso comune</span><span class="sxs-lookup"><span data-stu-id="24818-110">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="24818-111">Vedere [script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) per tutti gli attributi, le proprietà e i metodi dell'helper tag di script.</span><span class="sxs-lookup"><span data-stu-id="24818-111">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="asp-fallback-test"></a><span data-ttu-id="24818-112">ASP-fallback-test</span><span class="sxs-lookup"><span data-stu-id="24818-112">asp-fallback-test</span></span>

<span data-ttu-id="24818-113">Metodo di script definito nello script principale da usare per il test di fallback.</span><span class="sxs-lookup"><span data-stu-id="24818-113">The script method defined in the primary script to use for the fallback test.</span></span> <span data-ttu-id="24818-114">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span><span class="sxs-lookup"><span data-stu-id="24818-114">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span></span>

### <a name="asp-fallback-src"></a><span data-ttu-id="24818-115">ASP-fallback-src</span><span class="sxs-lookup"><span data-stu-id="24818-115">asp-fallback-src</span></span>

<span data-ttu-id="24818-116">L'URL di un tag di script di cui eseguire il fallback nel caso in cui si verifica l'errore primario.</span><span class="sxs-lookup"><span data-stu-id="24818-116">The URL of a Script tag to fallback to in the case the primary one fails.</span></span> <span data-ttu-id="24818-117">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span><span class="sxs-lookup"><span data-stu-id="24818-117">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24818-118">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="24818-118">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
