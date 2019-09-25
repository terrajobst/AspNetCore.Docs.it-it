---
title: Helper tag di script in ASP.NET Core
author: rick-anderson
ms.author: riande
description: Individuare gli attributi degli helper tag di script ASP.NET Core e il ruolo di ciascun attributo per estendere il comportamento del tag di script HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256498"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="fcb76-103">Helper tag di script in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcb76-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="fcb76-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fcb76-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fcb76-105">L' [Helper tag script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) genera un collegamento a un file di script primario o di fallback.</span><span class="sxs-lookup"><span data-stu-id="fcb76-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="fcb76-106">In genere il file di script primario si trova in una rete per la [distribuzione di contenuti](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="fcb76-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="fcb76-107">L'helper tag script consente di specificare una rete CDN per il file di script e un fallback quando la rete CDN non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="fcb76-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="fcb76-108">L'helper tag script offre il vantaggio in merito alle prestazioni di una rete CDN con l'affidabilità dell'hosting locale.</span><span class="sxs-lookup"><span data-stu-id="fcb76-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="fcb76-109">Il markup Razor seguente mostra l' `script` elemento di un file di layout creato con il modello di app Web di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fcb76-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="fcb76-110">L'esempio seguente è simile al codice HTML sottoposto a rendering del codice precedente (in un ambiente non di sviluppo):</span><span class="sxs-lookup"><span data-stu-id="fcb76-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="fcb76-111">Nel codice precedente, l'helper tag di script ha generato il secondo elemento script `<script>  (window.jQuery || document.write(`(), che `window.jQuery`verifica.</span><span class="sxs-lookup"><span data-stu-id="fcb76-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="fcb76-112">Se `window.jQuery` non viene trovato, `document.write(` esegue e crea uno script</span><span class="sxs-lookup"><span data-stu-id="fcb76-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="fcb76-113">Attributi di helper tag di script di uso comune</span><span class="sxs-lookup"><span data-stu-id="fcb76-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="fcb76-114">Vedere [script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) per tutti gli attributi, le proprietà e i metodi dell'helper tag di script.</span><span class="sxs-lookup"><span data-stu-id="fcb76-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="fcb76-115">href</span><span class="sxs-lookup"><span data-stu-id="fcb76-115">href</span></span>

<span data-ttu-id="fcb76-116">Indirizzo preferenziale della risorsa collegata.</span><span class="sxs-lookup"><span data-stu-id="fcb76-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="fcb76-117">All'indirizzo viene passato un pensiero al codice HTML generato in tutti i casi.</span><span class="sxs-lookup"><span data-stu-id="fcb76-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="fcb76-118">ASP-fallback-href</span><span class="sxs-lookup"><span data-stu-id="fcb76-118">asp-fallback-href</span></span>

<span data-ttu-id="fcb76-119">URL di un foglio di stile CSS di cui eseguire il fallback in caso di errore dell'URL primario.</span><span class="sxs-lookup"><span data-stu-id="fcb76-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="fcb76-120">ASP-fallback-test-Class</span><span class="sxs-lookup"><span data-stu-id="fcb76-120">asp-fallback-test-class</span></span>

<span data-ttu-id="fcb76-121">Nome della classe definito nel foglio di stile da utilizzare per il test di fallback.</span><span class="sxs-lookup"><span data-stu-id="fcb76-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="fcb76-122">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="fcb76-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="fcb76-123">ASP-fallback-test-proprietà</span><span class="sxs-lookup"><span data-stu-id="fcb76-123">asp-fallback-test-property</span></span>

<span data-ttu-id="fcb76-124">Nome della proprietà CSS da utilizzare per il test di fallback.</span><span class="sxs-lookup"><span data-stu-id="fcb76-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="fcb76-125">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="fcb76-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="fcb76-126">ASP-fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="fcb76-126">asp-fallback-test-value</span></span>

<span data-ttu-id="fcb76-127">Valore della proprietà CSS da utilizzare per il test di fallback.</span><span class="sxs-lookup"><span data-stu-id="fcb76-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="fcb76-128">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="fcb76-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="fcb76-129">ASP-fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="fcb76-129">asp-fallback-test-value</span></span>

<span data-ttu-id="fcb76-130">Valore della proprietà CSS da utilizzare per il test di fallback.</span><span class="sxs-lookup"><span data-stu-id="fcb76-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="fcb76-131">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="fcb76-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcb76-132">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fcb76-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
