---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web ASP.NET Core e sull'host generico .NET, responsabili della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276617"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="4d35b-103">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d35b-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="4d35b-104">Le app .NET configurano e avviano un *host*.</span><span class="sxs-lookup"><span data-stu-id="4d35b-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="4d35b-105">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="4d35b-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4d35b-106">Sono disponibili per l'uso due host API:</span><span class="sxs-lookup"><span data-stu-id="4d35b-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="4d35b-107">[Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="4d35b-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="4d35b-108">[Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background.</span><span class="sxs-lookup"><span data-stu-id="4d35b-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="4d35b-109">In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web.</span><span class="sxs-lookup"><span data-stu-id="4d35b-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="4d35b-110">L'host generico è destinato a sostituire l'host Web.</span><span class="sxs-lookup"><span data-stu-id="4d35b-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="4d35b-111">Per l'hosting delle *app Web* ASP.NET Core, gli sviluppatori devono usare l'host Web basato su [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4d35b-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="4d35b-112">Per l'hosting delle app *non Web*, gli sviluppatori devono usare l'host generico basato su [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4d35b-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
