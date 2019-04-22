---
title: Ospitare e distribuire Blazor sul lato server
author: guardrex
description: Informazioni su come ospitare e distribuire un'app sul lato server Blazor tramite ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614715"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="05192-103">Ospitare e distribuire Blazor sul lato server</span><span class="sxs-lookup"><span data-stu-id="05192-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="05192-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="05192-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="05192-105">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="05192-105">Host configuration values</span></span>

<span data-ttu-id="05192-106">Le app sul lato server che usano il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side-hosting-model) possono accettare [valori di configurazione dell'host generici](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="05192-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="05192-107">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="05192-107">Deployment</span></span>

<span data-ttu-id="05192-108">Con il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side-hosting-model), Blazor viene eseguito nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05192-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="05192-109">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="05192-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="05192-110">L'app è inclusa con l'app ASP.NET Core nell'output pubblicato e le due app vengono distribuite insieme.</span><span class="sxs-lookup"><span data-stu-id="05192-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="05192-111">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05192-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="05192-112">Per una distribuzione sul lato server, Visual Studio include il modello di progetto **Razor Components** (modello `razorcomponents` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="05192-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="05192-113">Per altre informazioni su hosting e distribuzione di app ASP.NET Core, vedere <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="05192-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="05192-114">Per informazioni sulla distribuzione in Servizio app di Azure, vedere <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="05192-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
