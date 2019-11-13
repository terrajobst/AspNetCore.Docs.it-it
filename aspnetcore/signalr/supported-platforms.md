---
title: Piattaforme supportate da ASP.NET Core SignalR
author: bradygaster
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 86ba5b1aec230d78c1a0e1709187e129df6cb4cc
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963734"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="74b90-103">Piattaforme supportate da ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="74b90-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="74b90-104">Requisiti di sistema del server</span><span class="sxs-lookup"><span data-stu-id="74b90-104">Server system requirements</span></span>

SignalR<span data-ttu-id="74b90-105"> per ASP.NET Core supporta qualsiasi piattaforma server supportata da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="74b90-105"> for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="74b90-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="74b90-106">JavaScript client</span></span>

<span data-ttu-id="74b90-107">Il [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) viene eseguito in NodeJS 8 e versioni successive e nei browser seguenti:</span><span class="sxs-lookup"><span data-stu-id="74b90-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="74b90-108">Browser</span><span class="sxs-lookup"><span data-stu-id="74b90-108">Browser</span></span>                         | <span data-ttu-id="74b90-109">Versione</span><span class="sxs-lookup"><span data-stu-id="74b90-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="74b90-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="74b90-110">Microsoft Edge</span></span>                  | <span data-ttu-id="74b90-111">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="74b90-111">Current&dagger;</span></span> |
| <span data-ttu-id="74b90-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="74b90-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="74b90-113">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="74b90-113">Current&dagger;</span></span> |
| <span data-ttu-id="74b90-114">Google Chrome; include Android</span><span class="sxs-lookup"><span data-stu-id="74b90-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="74b90-115">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="74b90-115">Current&dagger;</span></span> |
| <span data-ttu-id="74b90-116">Safari include iOS</span><span class="sxs-lookup"><span data-stu-id="74b90-116">Safari; includes iOS</span></span>            | <span data-ttu-id="74b90-117">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="74b90-117">Current&dagger;</span></span> |
| <span data-ttu-id="74b90-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="74b90-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="74b90-119">11</span><span class="sxs-lookup"><span data-stu-id="74b90-119">11</span></span>              |

<span data-ttu-id="74b90-120">&dagger;*corrente* si riferisce alla versione più recente del browser.</span><span class="sxs-lookup"><span data-stu-id="74b90-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="74b90-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="74b90-121">.NET client</span></span>

<span data-ttu-id="74b90-122">Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma supportata da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="74b90-122">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="74b90-123">Ad esempio, [gli sviluppatori Novell possono usare SignalR](https://github.com/aspnet/Announcements/issues/305) per creare app Android usando Novell. Android 8.4.0.1 e versioni successive e app iOS usando Novell. iOS 11.14.0.4 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="74b90-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="74b90-124">Se il server esegue IIS, il trasporto WebSocket richiede IIS 8,0 o versioni successive in Windows Server 2012 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="74b90-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="74b90-125">Gli altri trasporti sono supportati in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="74b90-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="74b90-126">Client Java</span><span class="sxs-lookup"><span data-stu-id="74b90-126">Java client</span></span>

<span data-ttu-id="74b90-127">Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="74b90-127">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="74b90-128">Client non supportati</span><span class="sxs-lookup"><span data-stu-id="74b90-128">Unsupported clients</span></span>

<span data-ttu-id="74b90-129">I client seguenti sono disponibili, ma sono sperimentali o non ufficiali.</span><span class="sxs-lookup"><span data-stu-id="74b90-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="74b90-130">Non sono attualmente supportati e potrebbero non essere mai.</span><span class="sxs-lookup"><span data-stu-id="74b90-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="74b90-131">[C++client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span><span class="sxs-lookup"><span data-stu-id="74b90-131">[C++ client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span></span>

* <span data-ttu-id="74b90-132">[Client Swift](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="74b90-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
