---
title: Piattaforme supportate da ASP.NET Core Blazor
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160132"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a><span data-ttu-id="61184-103">Piattaforme supportate da ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="61184-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="61184-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="61184-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a><span data-ttu-id="61184-105">Requisiti del browser</span><span class="sxs-lookup"><span data-stu-id="61184-105">Browser requirements</span></span>

### <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="61184-106"> webassembly</span><span class="sxs-lookup"><span data-stu-id="61184-106"> WebAssembly</span></span>

| <span data-ttu-id="61184-107">Browser</span><span class="sxs-lookup"><span data-stu-id="61184-107">Browser</span></span>                          | <span data-ttu-id="61184-108">Versione</span><span class="sxs-lookup"><span data-stu-id="61184-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="61184-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="61184-109">Microsoft Edge</span></span>                   | <span data-ttu-id="61184-110">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-110">Current</span></span>               |
| <span data-ttu-id="61184-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="61184-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="61184-112">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-112">Current</span></span>               |
| <span data-ttu-id="61184-113">Google Chrome, incluso Android</span><span class="sxs-lookup"><span data-stu-id="61184-113">Google Chrome, including Android</span></span> | <span data-ttu-id="61184-114">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-114">Current</span></span>               |
| <span data-ttu-id="61184-115">Safari, incluso iOS</span><span class="sxs-lookup"><span data-stu-id="61184-115">Safari, including iOS</span></span>            | <span data-ttu-id="61184-116">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-116">Current</span></span>               |
| <span data-ttu-id="61184-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="61184-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="61184-118">Non supportato&dagger;</span><span class="sxs-lookup"><span data-stu-id="61184-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="61184-119">&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).</span><span class="sxs-lookup"><span data-stu-id="61184-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="opno-locblazor-server"></a><span data-ttu-id="61184-120">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="61184-120">Blazor Server</span></span>

| <span data-ttu-id="61184-121">Browser</span><span class="sxs-lookup"><span data-stu-id="61184-121">Browser</span></span>                          | <span data-ttu-id="61184-122">Versione</span><span class="sxs-lookup"><span data-stu-id="61184-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="61184-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="61184-123">Microsoft Edge</span></span>                   | <span data-ttu-id="61184-124">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-124">Current</span></span>    |
| <span data-ttu-id="61184-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="61184-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="61184-126">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-126">Current</span></span>    |
| <span data-ttu-id="61184-127">Google Chrome, incluso Android</span><span class="sxs-lookup"><span data-stu-id="61184-127">Google Chrome, including Android</span></span> | <span data-ttu-id="61184-128">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-128">Current</span></span>    |
| <span data-ttu-id="61184-129">Safari, incluso iOS</span><span class="sxs-lookup"><span data-stu-id="61184-129">Safari, including iOS</span></span>            | <span data-ttu-id="61184-130">Corrente</span><span class="sxs-lookup"><span data-stu-id="61184-130">Current</span></span>    |
| <span data-ttu-id="61184-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="61184-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="61184-132">11&dagger;</span><span class="sxs-lookup"><span data-stu-id="61184-132">11&dagger;</span></span> |

<span data-ttu-id="61184-133">sono necessari &dagger;ulteriori riempimenti, ad esempio le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) .</span><span class="sxs-lookup"><span data-stu-id="61184-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61184-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="61184-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
