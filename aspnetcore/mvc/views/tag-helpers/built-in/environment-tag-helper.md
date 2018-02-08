---
title: Helper tag di ambiente in ASP.NET Core
author: pkellner
description: "Helper tag di ambiente ASP.NET Core definito con tutte le proprietà"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="e4132-103">Helper tag di ambiente in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4132-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="e4132-104">Di [Peter Kellner](http://peterkellner.net) e [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="e4132-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="e4132-105">L'helper tag di ambiente esegue il rendering condizionale del proprio contenuto in base all'ambiente host corrente.</span><span class="sxs-lookup"><span data-stu-id="e4132-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="e4132-106">L'unico attributo `names` dell'helper tag è un elenco di nomi di ambiente separati da virgole. Se uno qualsiasi di questi nomi corrisponde all'ambiente corrente, viene attivato il rendering del contenuto dell'helper tag.</span><span class="sxs-lookup"><span data-stu-id="e4132-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="e4132-107">Attributi dell'helper tag di ambiente</span><span class="sxs-lookup"><span data-stu-id="e4132-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="e4132-108">nomi</span><span class="sxs-lookup"><span data-stu-id="e4132-108">names</span></span>

<span data-ttu-id="e4132-109">Accetta un singolo nome di ambiente host o un elenco di nomi di ambiente separati da virgole, che attiva il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="e4132-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="e4132-110">Il o i valori vengono confrontati con il valore corrente restituito dalla proprietà statica `HostingEnvironment.EnvironmentName` di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4132-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="e4132-111">Il valore è uno dei seguenti: **Staging**, **Development** o **Production**.</span><span class="sxs-lookup"><span data-stu-id="e4132-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="e4132-112">Il confronto non applica la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="e4132-112">The comparison ignores case.</span></span>

<span data-ttu-id="e4132-113">Un esempio di helper tag `environment` valido è il seguente:</span><span class="sxs-lookup"><span data-stu-id="e4132-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="e4132-114">Attributi include ed exclude</span><span class="sxs-lookup"><span data-stu-id="e4132-114">include and exclude attributes</span></span>

<span data-ttu-id="e4132-115">In ASP.NET Core 2.x vengono aggiunti gli attributi `include` & `exclude`.</span><span class="sxs-lookup"><span data-stu-id="e4132-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="e4132-116">Questi attributi controllano il rendering del contenuto dell'helper tag in base ai nomi ambiente host inclusi o esclusi.</span><span class="sxs-lookup"><span data-stu-id="e4132-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="e4132-117">include (ASP.NET Core 2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e4132-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="e4132-118">La proprietà `include` ha un comportamento simile a quello dell'attributo `names` in ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="e4132-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="e4132-119">exclude (ASP.NET Core 2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e4132-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="e4132-120">Al contrario la proprietà `exclude` consente a `EnvironmentTagHelper` di eseguire il rendering del contenuto per tutti i nomi di ambiente host, ad eccezione di quelli specificati dall'utente.</span><span class="sxs-lookup"><span data-stu-id="e4132-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="e4132-121">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e4132-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
