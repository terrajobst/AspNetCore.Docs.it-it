---
title: Ospitare e distribuire Blazor sul lato server
author: guardrex
description: Informazioni su come ospitare e distribuire un'app sul lato server Blazor tramite ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887766"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="7218e-103">Ospitare e distribuire Blazor sul lato server</span><span class="sxs-lookup"><span data-stu-id="7218e-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="7218e-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7218e-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="7218e-105">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="7218e-105">Host configuration values</span></span>

<span data-ttu-id="7218e-106">Le app sul lato server che usano il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side) possono accettare [valori di configurazione dell'host generici](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="7218e-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="7218e-107">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="7218e-107">Deployment</span></span>

<span data-ttu-id="7218e-108">Con il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side), Blazor viene eseguito nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7218e-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="7218e-109">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="7218e-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="7218e-110">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7218e-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="7218e-111">Visual Studio include il modello di progetto **Bazor (lato server)** (modello `blazorserverside` se si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="7218e-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a><span data-ttu-id="7218e-112">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7218e-112">Additional resources</span></span>

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="7218e-113">Distribuire la versione di anteprima di ASP.NET Core nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="7218e-113">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
