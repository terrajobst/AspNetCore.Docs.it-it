---
title: Piattaforme supportate da ASP.NET Core Blazor
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962736"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a><span data-ttu-id="36670-103">Piattaforme supportate da ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="36670-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="36670-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="36670-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a><span data-ttu-id="36670-105">Requisiti del browser</span><span class="sxs-lookup"><span data-stu-id="36670-105">Browser requirements</span></span>

### <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="36670-106"> webassembly</span><span class="sxs-lookup"><span data-stu-id="36670-106"> WebAssembly</span></span>

| <span data-ttu-id="36670-107">Browser</span><span class="sxs-lookup"><span data-stu-id="36670-107">Browser</span></span>                          | <span data-ttu-id="36670-108">Versione</span><span class="sxs-lookup"><span data-stu-id="36670-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="36670-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="36670-109">Microsoft Edge</span></span>                   | <span data-ttu-id="36670-110">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-110">Current</span></span>               |
| <span data-ttu-id="36670-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="36670-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="36670-112">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-112">Current</span></span>               |
| <span data-ttu-id="36670-113">Google Chrome, incluso Android</span><span class="sxs-lookup"><span data-stu-id="36670-113">Google Chrome, including Android</span></span> | <span data-ttu-id="36670-114">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-114">Current</span></span>               |
| <span data-ttu-id="36670-115">Safari, incluso iOS</span><span class="sxs-lookup"><span data-stu-id="36670-115">Safari, including iOS</span></span>            | <span data-ttu-id="36670-116">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-116">Current</span></span>               |
| <span data-ttu-id="36670-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="36670-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="36670-118">Non supportato&dagger;</span><span class="sxs-lookup"><span data-stu-id="36670-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="36670-119">&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).</span><span class="sxs-lookup"><span data-stu-id="36670-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="opno-locblazor-server"></a><span data-ttu-id="36670-120">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="36670-120">Blazor Server</span></span>

| <span data-ttu-id="36670-121">Browser</span><span class="sxs-lookup"><span data-stu-id="36670-121">Browser</span></span>                          | <span data-ttu-id="36670-122">Versione</span><span class="sxs-lookup"><span data-stu-id="36670-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="36670-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="36670-123">Microsoft Edge</span></span>                   | <span data-ttu-id="36670-124">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-124">Current</span></span>    |
| <span data-ttu-id="36670-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="36670-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="36670-126">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-126">Current</span></span>    |
| <span data-ttu-id="36670-127">Google Chrome, incluso Android</span><span class="sxs-lookup"><span data-stu-id="36670-127">Google Chrome, including Android</span></span> | <span data-ttu-id="36670-128">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-128">Current</span></span>    |
| <span data-ttu-id="36670-129">Safari, incluso iOS</span><span class="sxs-lookup"><span data-stu-id="36670-129">Safari, including iOS</span></span>            | <span data-ttu-id="36670-130">Corrente</span><span class="sxs-lookup"><span data-stu-id="36670-130">Current</span></span>    |
| <span data-ttu-id="36670-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="36670-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="36670-132">11&dagger;</span><span class="sxs-lookup"><span data-stu-id="36670-132">11&dagger;</span></span> |

<span data-ttu-id="36670-133">sono necessarie le compilazioni &dagger;Additional (ad esempio, le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) ).</span><span class="sxs-lookup"><span data-stu-id="36670-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36670-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="36670-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
