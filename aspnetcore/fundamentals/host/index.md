---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web ASP.NET Core e sull'host generico .NET, responsabili della gestione dell'avvio e della durata delle app.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="f114a-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f114a-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="f114a-104">Le app .NET configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="f114a-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="f114a-105">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="f114a-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f114a-106">Sono disponibili per l'uso due host API:</span><span class="sxs-lookup"><span data-stu-id="f114a-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="f114a-107">[Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="f114a-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="f114a-108">[Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background.</span><span class="sxs-lookup"><span data-stu-id="f114a-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="f114a-109">In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web.</span><span class="sxs-lookup"><span data-stu-id="f114a-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="f114a-110">L'host generico è destinato a sostituire l'host Web.</span><span class="sxs-lookup"><span data-stu-id="f114a-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="f114a-111">Al momento, gli sviluppatori devono usare l'[host Web](xref:fundamentals/host/web-host) basato su [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) per l'hosting di app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f114a-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
