---
title: Piattaforme supportate da ASP.NET Core SignalR
author: bradygaster
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 054965921c87c1a9be27e5ddaa8a87b0fa1f4113
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78668140"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="bbb07-103">Piattaforme supportate da ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bbb07-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="bbb07-104">Requisiti di sistema del server di</span><span class="sxs-lookup"><span data-stu-id="bbb07-104">Server system requirements</span></span>

<span data-ttu-id="bbb07-105">SignalR per ASP.NET Core supporta qualsiasi piattaforma server supportata da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbb07-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="bbb07-106">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="bbb07-106">JavaScript client</span></span>

<span data-ttu-id="bbb07-107">Il [client JavaScript](xref:signalr/javascript-client) viene eseguito in NodeJS 8 e versioni successive e nei browser seguenti:</span><span class="sxs-lookup"><span data-stu-id="bbb07-107">The [JavaScript client](xref:signalr/javascript-client) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="bbb07-108">Browser</span><span class="sxs-lookup"><span data-stu-id="bbb07-108">Browser</span></span>                         | <span data-ttu-id="bbb07-109">Versione</span><span class="sxs-lookup"><span data-stu-id="bbb07-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="bbb07-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="bbb07-110">Microsoft Edge</span></span>                  | <span data-ttu-id="bbb07-111">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="bbb07-111">Current&dagger;</span></span> |
| <span data-ttu-id="bbb07-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="bbb07-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="bbb07-113">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="bbb07-113">Current&dagger;</span></span> |
| <span data-ttu-id="bbb07-114">Google Chrome; include Android</span><span class="sxs-lookup"><span data-stu-id="bbb07-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="bbb07-115">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="bbb07-115">Current&dagger;</span></span> |
| <span data-ttu-id="bbb07-116">Safari include iOS</span><span class="sxs-lookup"><span data-stu-id="bbb07-116">Safari; includes iOS</span></span>            | <span data-ttu-id="bbb07-117">&dagger; corrente</span><span class="sxs-lookup"><span data-stu-id="bbb07-117">Current&dagger;</span></span> |
| <span data-ttu-id="bbb07-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="bbb07-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="bbb07-119">11</span><span class="sxs-lookup"><span data-stu-id="bbb07-119">11</span></span>              |

<span data-ttu-id="bbb07-120">&dagger;*corrente* si riferisce alla versione più recente del browser.</span><span class="sxs-lookup"><span data-stu-id="bbb07-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="bbb07-121">Client .NET</span><span class="sxs-lookup"><span data-stu-id="bbb07-121">.NET client</span></span>

<span data-ttu-id="bbb07-122">Il [client .NET](xref:signalr/dotnet-client) viene eseguito su qualsiasi piattaforma supportata da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bbb07-122">The [.NET client](xref:signalr/dotnet-client) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="bbb07-123">Ad esempio, [gli sviluppatori Novell possono usare SignalR](https://github.com/aspnet/Announcements/issues/305) per creare app Android usando Novell. Android 8.4.0.1 e versioni successive e app iOS usando Novell. iOS 11.14.0.4 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="bbb07-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="bbb07-124">Se il server esegue IIS, il trasporto WebSocket richiede IIS 8,0 o versioni successive in Windows Server 2012 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="bbb07-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="bbb07-125">Gli altri trasporti sono supportati in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="bbb07-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="bbb07-126">Client Java</span><span class="sxs-lookup"><span data-stu-id="bbb07-126">Java client</span></span>

<span data-ttu-id="bbb07-127">Il [client Java](xref:signalr/java-client) supporta Java 8 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="bbb07-127">The [Java client](xref:signalr/java-client) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="bbb07-128">Client non supportati</span><span class="sxs-lookup"><span data-stu-id="bbb07-128">Unsupported clients</span></span>

<span data-ttu-id="bbb07-129">I client seguenti sono disponibili, ma sono sperimentali o non ufficiali.</span><span class="sxs-lookup"><span data-stu-id="bbb07-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="bbb07-130">Non sono attualmente supportati e potrebbero non essere mai.</span><span class="sxs-lookup"><span data-stu-id="bbb07-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="bbb07-131">[C++client](https://github.com/aspnet/SignalR-Client-Cpp)</span><span class="sxs-lookup"><span data-stu-id="bbb07-131">[C++ client](https://github.com/aspnet/SignalR-Client-Cpp)</span></span>

* <span data-ttu-id="bbb07-132">[Client Swift](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="bbb07-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
