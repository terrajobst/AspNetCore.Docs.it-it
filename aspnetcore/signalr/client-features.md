---
title: Funzionalità del client SignalR
author: bradygaster
description: Informazioni sulle funzionalità supportate dai diversi client di ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301182"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="d170d-103">Funzionalità client di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d170d-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="d170d-104">Distribuzione di funzionalità</span><span class="sxs-lookup"><span data-stu-id="d170d-104">Feature distribution</span></span>

<span data-ttu-id="d170d-105">La tabella seguente illustra le funzionalità e il supporto per i client che offrono il supporto in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="d170d-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="d170d-106">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="d170d-106">Feature</span></span> | <span data-ttu-id="d170d-107">.NET</span><span class="sxs-lookup"><span data-stu-id="d170d-107">.NET</span></span> | <span data-ttu-id="d170d-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d170d-108">JavaScript</span></span> | <span data-ttu-id="d170d-109">Java</span><span class="sxs-lookup"><span data-stu-id="d170d-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="d170d-110">Supporto del servizio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="d170d-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="d170d-111">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-111">✔</span></span>|<span data-ttu-id="d170d-112">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-112">✔</span></span>|<span data-ttu-id="d170d-113">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-113">✔</span></span>|
| [<span data-ttu-id="d170d-114">Streaming da server a client</span><span class="sxs-lookup"><span data-stu-id="d170d-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="d170d-115">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-115">✔</span></span>|<span data-ttu-id="d170d-116">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-116">✔</span></span>|<span data-ttu-id="d170d-117">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-117">✔</span></span>|
| [<span data-ttu-id="d170d-118">Streaming da client a server</span><span class="sxs-lookup"><span data-stu-id="d170d-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="d170d-119">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-119">✔</span></span>|<span data-ttu-id="d170d-120">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-120">✔</span></span>|<span data-ttu-id="d170d-121">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-121">✔</span></span>|
| <span data-ttu-id="d170d-122">Riconnessione automatica ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="d170d-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="d170d-123">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-123">✔</span></span>|<span data-ttu-id="d170d-124">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-124">✔</span></span>| |
| <span data-ttu-id="d170d-125">Trasporto WebSocket</span><span class="sxs-lookup"><span data-stu-id="d170d-125">WebSockets Transport</span></span> |<span data-ttu-id="d170d-126">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-126">✔</span></span>|<span data-ttu-id="d170d-127">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-127">✔</span></span>|<span data-ttu-id="d170d-128">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-128">✔</span></span>|
| <span data-ttu-id="d170d-129">Trasporto eventi inviati dal server</span><span class="sxs-lookup"><span data-stu-id="d170d-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="d170d-130">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-130">✔</span></span>|<span data-ttu-id="d170d-131">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-131">✔</span></span>| |
| <span data-ttu-id="d170d-132">Trasporto di polling prolungato</span><span class="sxs-lookup"><span data-stu-id="d170d-132">Long Polling Transport</span></span> |<span data-ttu-id="d170d-133">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-133">✔</span></span>|<span data-ttu-id="d170d-134">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-134">✔</span></span>|<span data-ttu-id="d170d-135">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-135">✔</span></span>|
| <span data-ttu-id="d170d-136">Protocollo dell'hub JSON</span><span class="sxs-lookup"><span data-stu-id="d170d-136">JSON Hub Protocol</span></span> |<span data-ttu-id="d170d-137">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-137">✔</span></span>|<span data-ttu-id="d170d-138">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-138">✔</span></span>|<span data-ttu-id="d170d-139">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-139">✔</span></span>|
| <span data-ttu-id="d170d-140">Protocollo hub MessagePack</span><span class="sxs-lookup"><span data-stu-id="d170d-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="d170d-141">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-141">✔</span></span>|<span data-ttu-id="d170d-142">✔</span><span class="sxs-lookup"><span data-stu-id="d170d-142">✔</span></span>| |

<span data-ttu-id="d170d-143">Il supporto per la riconnessione automatica nel client Java viene registrato nello strumento di [rilevamento dei problemi](https://github.com/aspnet/AspNetCore/issues/8711).</span><span class="sxs-lookup"><span data-stu-id="d170d-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d170d-144">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d170d-144">Additional resources</span></span>

* [<span data-ttu-id="d170d-145">Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d170d-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="d170d-146">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="d170d-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="d170d-147">Hub</span><span class="sxs-lookup"><span data-stu-id="d170d-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d170d-148">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="d170d-148">JavaScript client</span></span>](xref:signalr/javascript-client)
