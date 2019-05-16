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
# <a name="host-and-deploy-blazor-server-side"></a>Ospitare e distribuire Blazor sul lato server

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valori di configurazione dell'host

Le app sul lato server che usano il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side) possono accettare [valori di configurazione dell'host generici](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Distribuzione

Con il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side), Blazor viene eseguito nel server dall'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).

È necessario un server Web in grado di ospitare un'app ASP.NET Core. Visual Studio include il modello di progetto **Bazor (lato server)** (modello `blazorserverside` se si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Distribuire la versione di anteprima di ASP.NET Core nel servizio app di Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
