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
# <a name="host-and-deploy-blazor-server-side"></a>Ospitare e distribuire Blazor sul lato server

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valori di configurazione dell'host

Le app sul lato server che usano il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side) possono accettare [valori di configurazione dell'host generici](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Distribuzione

Con il [modello di hosting sul lato server](xref:blazor/hosting-models#server-side), Blazor viene eseguito nel server dall'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).

È necessario un server Web in grado di ospitare un'app ASP.NET Core. Visual Studio include il modello di progetto **App server Blazor** (modello `blazorserverside` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="connection-scale-out"></a>Ridimensionamento delle connessioni

Le app sul lato server Blazor richiedono una connessione SignalR attiva per ogni utente. Una distribuzione sul lato server di Blazor di produzione richiede una soluzione per supportare il numero di connessioni simultanee richiesto dall'app. Il [servizio Azure SignalR](/azure/azure-signalr/) gestisce il ridimensionamento delle connessioni ed è consigliato come soluzione di ridimensionamento per le app sul lato server Blazor. Per altre informazioni, vedere <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>Configurazione di SignalR

SignalR viene configurato da ASP.NET Core per gli scenari sul lato server di Blazor più comuni. Per gli scenari personalizzati e avanzati, consultare gli articoli dedicati a SignalR nella sezione [Risorse aggiuntive](#additional-resources).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/introduction>
* [Documentazione del servizio Azure SignalR](/azure/azure-signalr/)
* [Avvio rapido: Creare una chat room mediante il servizio SignalR](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Distribuire la versione di anteprima di ASP.NET Core nel servizio app di Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
