---
title: Ospitare e distribuire ASP.NET Core Blazor sul lato server
author: guardrex
description: Informazioni su come ospitare e distribuire un'app sul lato server Blazor tramite ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 56a03ff583bf85497e2b3bacc70123845a046e3d
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "67892693"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="efd0b-103">Ospitare e distribuire Blazor sul lato server</span><span class="sxs-lookup"><span data-stu-id="efd0b-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="efd0b-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="efd0b-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="efd0b-105">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="efd0b-105">Host configuration values</span></span>

<span data-ttu-id="efd0b-106">Le app sul lato server che usano il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side) possono accettare [valori di configurazione dell'host generici](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="efd0b-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="efd0b-107">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="efd0b-107">Deployment</span></span>

<span data-ttu-id="efd0b-108">Con il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side), Blazor viene eseguito nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="efd0b-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="efd0b-109">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="efd0b-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="efd0b-110">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="efd0b-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="efd0b-111">Visual Studio include il modello di progetto **App server Blazor** (modello `blazorserverside` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="efd0b-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="efd0b-112">Ridimensionamento delle connessioni</span><span class="sxs-lookup"><span data-stu-id="efd0b-112">Connection scale out</span></span>

<span data-ttu-id="efd0b-113">Le app sul lato server Blazor richiedono una connessione SignalR attiva per ogni utente.</span><span class="sxs-lookup"><span data-stu-id="efd0b-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="efd0b-114">Una distribuzione sul lato server di Blazor di produzione richiede una soluzione per supportare il numero di connessioni simultanee richiesto dall'app.</span><span class="sxs-lookup"><span data-stu-id="efd0b-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="efd0b-115">Il [servizio Azure SignalR](/azure/azure-signalr/) gestisce il ridimensionamento delle connessioni ed è consigliato come soluzione di ridimensionamento per le app sul lato server Blazor.</span><span class="sxs-lookup"><span data-stu-id="efd0b-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="efd0b-116">Per altre informazioni, vedere <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="efd0b-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="efd0b-117">Configurazione di SignalR</span><span class="sxs-lookup"><span data-stu-id="efd0b-117">SignalR configuration</span></span>

<span data-ttu-id="efd0b-118">SignalR viene configurato da ASP.NET Core per gli scenari sul lato server di Blazor più comuni.</span><span class="sxs-lookup"><span data-stu-id="efd0b-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="efd0b-119">Per gli scenari personalizzati e avanzati, consultare gli articoli dedicati a SignalR nella sezione [Risorse aggiuntive](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="efd0b-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="efd0b-120">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="efd0b-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="efd0b-121">Documentazione del servizio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="efd0b-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="efd0b-122">Avvio rapido: Creare una chat room mediante il servizio SignalR</span><span class="sxs-lookup"><span data-stu-id="efd0b-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="efd0b-123">Distribuire la versione di anteprima di ASP.NET Core nel servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="efd0b-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
