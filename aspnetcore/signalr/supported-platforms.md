---
title: Piattaforme supportate di ASP.NET Core SignalR
author: tdykstra
description: Piattaforme supportate per ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577626"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="e318e-103">Piattaforme supportate di ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e318e-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="e318e-104">Requisiti di sistema di server</span><span class="sxs-lookup"><span data-stu-id="e318e-104">Server system requirements</span></span>

<span data-ttu-id="e318e-105">SignalR per ASP.NET Core supporta qualsiasi piattaforma del server di che ASP.NET Core supporta.</span><span class="sxs-lookup"><span data-stu-id="e318e-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="e318e-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="e318e-106">JavaScript client</span></span>

<span data-ttu-id="e318e-107">Il [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) viene eseguito su NodeJS 8 e versioni successive e i browser seguenti:</span><span class="sxs-lookup"><span data-stu-id="e318e-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="e318e-108">Browser</span><span class="sxs-lookup"><span data-stu-id="e318e-108">Browser</span></span> | <span data-ttu-id="e318e-109">Versione</span><span class="sxs-lookup"><span data-stu-id="e318e-109">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="e318e-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="e318e-110">Microsoft Edge</span></span> | <span data-ttu-id="e318e-111">corrente</span><span class="sxs-lookup"><span data-stu-id="e318e-111">current</span></span> |
| <span data-ttu-id="e318e-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="e318e-112">Mozilla Firefox</span></span> | <span data-ttu-id="e318e-113">corrente</span><span class="sxs-lookup"><span data-stu-id="e318e-113">current</span></span> |
| <span data-ttu-id="e318e-114">Google Chrome; include Android</span><span class="sxs-lookup"><span data-stu-id="e318e-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="e318e-115">corrente</span><span class="sxs-lookup"><span data-stu-id="e318e-115">current</span></span> |
| <span data-ttu-id="e318e-116">Safari; include iOS</span><span class="sxs-lookup"><span data-stu-id="e318e-116">Safari; includes iOS</span></span> | <span data-ttu-id="e318e-117">corrente</span><span class="sxs-lookup"><span data-stu-id="e318e-117">current</span></span> |
| <span data-ttu-id="e318e-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="e318e-118">Microsoft Internet Explorer</span></span> | <span data-ttu-id="e318e-119">11</span><span class="sxs-lookup"><span data-stu-id="e318e-119">11</span></span> |
 
## <a name="net-client"></a><span data-ttu-id="e318e-120">Client .NET</span><span class="sxs-lookup"><span data-stu-id="e318e-120">.NET client</span></span>

<span data-ttu-id="e318e-121">Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma di server supportato da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e318e-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="e318e-122">Quando il server in esecuzione IIS, il trasporto WebSocket richiede IIS 8.0 o versione successiva, in Windows Server 2012 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="e318e-122">When the server runs IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="e318e-123">Sono supportati altri tipi di trasporto in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="e318e-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="e318e-124">Client Java</span><span class="sxs-lookup"><span data-stu-id="e318e-124">Java client</span></span>

<span data-ttu-id="e318e-125">Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="e318e-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="e318e-126">Client non supportati</span><span class="sxs-lookup"><span data-stu-id="e318e-126">Unsupported clients</span></span>

<span data-ttu-id="e318e-127">I client seguenti sono disponibili ma sono sperimentali o non ufficiali.</span><span class="sxs-lookup"><span data-stu-id="e318e-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="e318e-128">Essi non sono ora supportate e non può mai essere supportati.</span><span class="sxs-lookup"><span data-stu-id="e318e-128">They are not supported now and may not ever be supported.</span></span>

* [<span data-ttu-id="e318e-129">Client C++</span><span class="sxs-lookup"><span data-stu-id="e318e-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="e318e-130">Client SWIFT</span><span class="sxs-lookup"><span data-stu-id="e318e-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
