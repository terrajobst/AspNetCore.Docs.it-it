---
title: Piattaforme supportate di ASP.NET Core SignalR
author: tdykstra
description: Piattaforme supportate per ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577626"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Piattaforme supportate di ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Requisiti di sistema di server

SignalR per ASP.NET Core supporta qualsiasi piattaforma del server di che ASP.NET Core supporta.

## <a name="javascript-client"></a>Client JavaScript

Il [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) viene eseguito su NodeJS 8 e versioni successive e i browser seguenti:

| Browser | Versione |
| ------- | ------- |
| Microsoft Edge | corrente |
| Mozilla Firefox | corrente |
| Google Chrome; include Android | corrente |
| Safari; include iOS | corrente |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>Client .NET

Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma di server supportato da ASP.NET Core.

Quando il server in esecuzione IIS, il trasporto WebSocket richiede IIS 8.0 o versione successiva, in Windows Server 2012 o versione successiva. Sono supportati altri tipi di trasporto in tutte le piattaforme.

## <a name="java-client"></a>Client Java

Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.

## <a name="unsupported-clients"></a>Client non supportati

I client seguenti sono disponibili ma sono sperimentali o non ufficiali. Essi non sono ora supportate e non può mai essere supportati.

* [Client C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
