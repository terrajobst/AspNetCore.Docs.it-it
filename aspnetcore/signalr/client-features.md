---
title: Funzionalità del client SignalR
author: bradygaster
description: Informazioni sulle funzionalità supportate dai diversi client di ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925349"
---
# <a name="aspnet-core-signalr-client-features"></a>Funzionalità client di ASP.NET Core SignalR

## <a name="feature-distribution"></a>Distribuzione di funzionalità

La tabella seguente illustra le funzionalità e il supporto per i client che offrono il supporto in tempo reale. Per ogni funzionalità viene elencata la versione *minima* supportata per questa funzionalità. Se non è elencata alcuna versione, la funzionalità non è supportata.

| Funzionalità | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Supporto del servizio Azure SignalR |1.0.0|1.0.0|1.0.0|
| [Streaming da server a client](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [Streaming da client a server](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| Riconnessione automatica ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |3.0.0|3.0.0|❌|
| Trasporto WebSocket |1.0.0|1.0.0|1.0.0|
| Trasporto eventi inviati dal server |1.0.0|1.0.0|❌|
| Trasporto di polling prolungato |1.0.0|1.0.0|3.0.0|
| Protocollo dell'hub JSON |1.0.0|1.0.0|1.0.0|
| Protocollo hub MessagePack |1.0.0|1.0.0|❌|

Il supporto per la riconnessione automatica nel client Java viene registrato nello strumento di [rilevamento dei problemi](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione a SignalR per ASP.NET Core](xref:tutorials/signalr)
* [Piattaforme supportate](xref:signalr/supported-platforms)
* [Hub](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
