---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web ASP.NET Core e sull'host generico .NET, responsabili della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336050"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="526f0-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="526f0-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="526f0-104">Le app .NET configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="526f0-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="526f0-105">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="526f0-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="526f0-106">Sono disponibili per l'uso due host API:</span><span class="sxs-lookup"><span data-stu-id="526f0-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="526f0-107">[Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="526f0-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="526f0-108">[Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background.</span><span class="sxs-lookup"><span data-stu-id="526f0-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="526f0-109">In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web.</span><span class="sxs-lookup"><span data-stu-id="526f0-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="526f0-110">L'host generico è destinato a sostituire l'host Web.</span><span class="sxs-lookup"><span data-stu-id="526f0-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="526f0-111">Per l'hosting delle *app Web* ASP.NET Core, gli sviluppatori devono usare l'host Web basato su <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="526f0-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="526f0-112">Per l'hosting delle *app non Web*, gli sviluppatori devono usare l'host generico basato su <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="526f0-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="526f0-113">Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="526f0-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="526f0-114">Informazioni su come migliorare un'app ASP.NET Core da un assembly con o senza riferimenti usando un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.</span><span class="sxs-lookup"><span data-stu-id="526f0-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
