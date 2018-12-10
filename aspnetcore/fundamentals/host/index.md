---
title: Host Web e host generico in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web ASP.NET Core e sull'host generico .NET, responsabili della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121518"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="7581e-103">Host Web e host generico in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7581e-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="7581e-104">Le app .NET configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="7581e-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="7581e-105">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="7581e-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7581e-106">Sono disponibili per l'uso due host API:</span><span class="sxs-lookup"><span data-stu-id="7581e-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="7581e-107">[Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="7581e-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="7581e-108">[Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background.</span><span class="sxs-lookup"><span data-stu-id="7581e-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="7581e-109">In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web.</span><span class="sxs-lookup"><span data-stu-id="7581e-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="7581e-110">L'host generico è destinato a sostituire l'host Web.</span><span class="sxs-lookup"><span data-stu-id="7581e-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="7581e-111">Per l'hosting delle *app Web* ASP.NET Core, gli sviluppatori devono usare l'host Web basato su <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7581e-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="7581e-112">Per l'hosting delle *app non Web*, gli sviluppatori devono usare l'host generico basato su <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7581e-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="7581e-113">Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7581e-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="7581e-114">Informazioni su come migliorare un'app ASP.NET Core da un assembly con o senza riferimenti usando un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.</span><span class="sxs-lookup"><span data-stu-id="7581e-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
