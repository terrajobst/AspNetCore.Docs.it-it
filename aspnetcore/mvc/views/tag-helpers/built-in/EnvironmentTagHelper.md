---
title: Helper di Tag di ambiente in ASP.NET Core
author: pkellner
description: "Helper di Tag di ambiente ASP.Net Core definito tra tutte le proprietà"
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: e16f942b4a42ef495c7a7daff6724ff071bb35a1
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="0244a-104">Helper di Tag di ambiente in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0244a-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="0244a-105">Da [Peter Kellner](http://peterkellner.net) e [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="0244a-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="0244a-106">L'Helper di Tag di ambiente in modo condizionale esegue il rendering di contenuto incluso in base all'ambiente di hosting corrente.</span><span class="sxs-lookup"><span data-stu-id="0244a-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="0244a-107">L'unico attributo `names` è un elenco separato da virgole di ambiente di nomi, che se qualsiasi corrispondono all'ambiente corrente, verrà attivato il contenuto incluso da sottoporre a rendering.</span><span class="sxs-lookup"><span data-stu-id="0244a-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="0244a-108">Attributi Helper di Tag di ambiente</span><span class="sxs-lookup"><span data-stu-id="0244a-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="0244a-109">nomi</span><span class="sxs-lookup"><span data-stu-id="0244a-109">names</span></span>

<span data-ttu-id="0244a-110">Accetta un singolo nome di ambiente host o un elenco delimitato da virgole di nomi di ambiente che attivano il rendering del contenuto tra parentesi di hosting.</span><span class="sxs-lookup"><span data-stu-id="0244a-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="0244a-111">Questi valori vengono confrontati con il valore corrente restituito dalla proprietà statica ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="0244a-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="0244a-112">Questo valore è uno dei seguenti: **gestione temporanea**; **Sviluppo** o **produzione**.</span><span class="sxs-lookup"><span data-stu-id="0244a-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="0244a-113">Ignora il confronto.</span><span class="sxs-lookup"><span data-stu-id="0244a-113">The comparison ignores case.</span></span>

<span data-ttu-id="0244a-114">Un esempio di un oggetto valido `environment` helper di tag è:</span><span class="sxs-lookup"><span data-stu-id="0244a-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="0244a-115">includere ed escludere gli attributi</span><span class="sxs-lookup"><span data-stu-id="0244a-115">include and exclude attributes</span></span>

<span data-ttu-id="0244a-116">ASP.NET Core 2. x aggiunge il `include`  &  `exclude` attributi.</span><span class="sxs-lookup"><span data-stu-id="0244a-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="0244a-117">Questi attributi controllano il rendering del contenuto tra parentesi, in base ai nomi ambiente hosting incluso o escluso.</span><span class="sxs-lookup"><span data-stu-id="0244a-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="0244a-118">includere Core ASP.NET 2.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="0244a-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="0244a-119">Il `include` proprietà presenta un comportamento simile del `names` attributo nella versione 1.0 di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0244a-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="0244a-120">escludere i Core ASP.NET 2.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="0244a-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="0244a-121">Al contrario, il `exclude` proprietà consente di `EnvironmentTagHelper` il rendering del contenuto per tutti i nomi di ambiente host, ad eccezione di indicare che è stato specificato tra parentesi.</span><span class="sxs-lookup"><span data-stu-id="0244a-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="0244a-122">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0244a-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
