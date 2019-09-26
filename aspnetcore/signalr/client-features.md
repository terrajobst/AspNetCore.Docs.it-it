---
title: Funzionalità del client SignalR
author: bradygaster
description: Informazioni sulle funzionalità supportate dai diversi client di ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301182"
---
# <a name="aspnet-core-signalr-client-features"></a>Funzionalità client di ASP.NET Core SignalR

## <a name="feature-distribution"></a>Distribuzione di funzionalità

La tabella seguente illustra le funzionalità e il supporto per i client che offrono il supporto in tempo reale.

| Funzionalità | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Supporto del servizio Azure SignalR |✔|✔|✔|
| [Streaming da server a client](xref:signalr/streaming)          |✔|✔|✔|
| [Streaming da client a server](xref:signalr/streaming)          |✔|✔|✔|
| Riconnessione automatica ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |
| Trasporto WebSocket |✔|✔|✔|
| Trasporto eventi inviati dal server |✔|✔| |
| Trasporto di polling prolungato |✔|✔|✔|
| Protocollo dell'hub JSON |✔|✔|✔|
| Protocollo hub MessagePack |✔|✔| |

Il supporto per la riconnessione automatica nel client Java viene registrato nello strumento di [rilevamento dei problemi](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione a SignalR per ASP.NET Core](xref:tutorials/signalr)
* [Piattaforme supportate](xref:signalr/supported-platforms)
* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
