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
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252009"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="364f3-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="364f3-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="364f3-104">Le app .NET configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="364f3-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="364f3-105">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="364f3-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="364f3-106">Sono disponibili per l'uso due host API:</span><span class="sxs-lookup"><span data-stu-id="364f3-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="364f3-107">[Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="364f3-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="364f3-108">[Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background.</span><span class="sxs-lookup"><span data-stu-id="364f3-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="364f3-109">In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web.</span><span class="sxs-lookup"><span data-stu-id="364f3-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="364f3-110">L'host generico è destinato a sostituire l'host Web.</span><span class="sxs-lookup"><span data-stu-id="364f3-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="364f3-111">Per l'hosting delle *app Web* ASP.NET Core, gli sviluppatori devono usare l'host Web basato su [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="364f3-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="364f3-112">Per l'hosting delle app *non Web*, gli sviluppatori devono usare l'host generico basato su [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="364f3-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
