---
title: Helper di Tag di ambiente in ASP.NET Core
author: pkellner
description: "Helper di Tag di ambiente ASP.NET Core definito tra tutte le proprietà"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="14adf-103">Helper di Tag di ambiente in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14adf-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="14adf-104">Da [Peter Kellner](http://peterkellner.net) e [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="14adf-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="14adf-105">L'Helper di Tag di ambiente in modo condizionale esegue il rendering di contenuto incluso in base all'ambiente di hosting corrente.</span><span class="sxs-lookup"><span data-stu-id="14adf-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="14adf-106">L'unico attributo `names` è un elenco separato da virgole di ambiente di nomi, che se qualsiasi corrispondono all'ambiente corrente, verrà attivato il contenuto incluso da sottoporre a rendering.</span><span class="sxs-lookup"><span data-stu-id="14adf-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="14adf-107">Attributi Helper di Tag di ambiente</span><span class="sxs-lookup"><span data-stu-id="14adf-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="14adf-108">nomi</span><span class="sxs-lookup"><span data-stu-id="14adf-108">names</span></span>

<span data-ttu-id="14adf-109">Accetta un singolo nome di ambiente host o un elenco delimitato da virgole di nomi di ambiente che attivano il rendering del contenuto tra parentesi di hosting.</span><span class="sxs-lookup"><span data-stu-id="14adf-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="14adf-110">Questi valori vengono confrontati con il valore corrente restituito dalla proprietà statica ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="14adf-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="14adf-111">Questo valore è uno dei seguenti: **gestione temporanea**; **Sviluppo** o **produzione**.</span><span class="sxs-lookup"><span data-stu-id="14adf-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="14adf-112">Ignora il confronto.</span><span class="sxs-lookup"><span data-stu-id="14adf-112">The comparison ignores case.</span></span>

<span data-ttu-id="14adf-113">Un esempio di un oggetto valido `environment` helper di tag è:</span><span class="sxs-lookup"><span data-stu-id="14adf-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="14adf-114">includere ed escludere gli attributi</span><span class="sxs-lookup"><span data-stu-id="14adf-114">include and exclude attributes</span></span>

<span data-ttu-id="14adf-115">ASP.NET Core 2. x aggiunge il `include`  &  `exclude` attributi.</span><span class="sxs-lookup"><span data-stu-id="14adf-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="14adf-116">Questi attributi controllano il rendering del contenuto tra parentesi, in base ai nomi ambiente hosting incluso o escluso.</span><span class="sxs-lookup"><span data-stu-id="14adf-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="14adf-117">includere Core ASP.NET 2.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="14adf-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="14adf-118">Il `include` proprietà presenta un comportamento simile del `names` attributo nella versione 1.0 di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14adf-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="14adf-119">escludere i Core ASP.NET 2.0 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="14adf-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="14adf-120">Al contrario, il `exclude` proprietà consente di `EnvironmentTagHelper` il rendering del contenuto per tutti i nomi di ambiente host, ad eccezione di indicare che è stato specificato tra parentesi.</span><span class="sxs-lookup"><span data-stu-id="14adf-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="14adf-121">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="14adf-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
