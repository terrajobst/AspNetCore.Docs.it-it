---
title: Piattaforme supportate di ASP.NET Core SignalR
author: tdykstra
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758180"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Piattaforme supportate di ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Requisiti di sistema di server

SignalR per ASP.NET Core supporta qualsiasi piattaforma di server che supporta ASP.NET Core.

## <a name="javascript-client"></a>Client JavaScript

Il [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) viene eseguito su NodeJS 8 e versioni successive e i browser seguenti:

| Browser                         | Versione |
| ------------------------------- | ------- |
| Microsoft Edge                  | corrente |
| Mozilla Firefox                 | corrente |
| Google Chrome; include Android | corrente |
| Safari; include iOS            | corrente |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Client .NET

Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma di server supportato da ASP.NET Core.

Se il server è in esecuzione IIS, il trasporto WebSocket richiede IIS 8.0 o versione successiva in Windows Server 2012 o versione successiva. Sono supportati altri tipi di trasporto in tutte le piattaforme.

## <a name="java-client"></a>Client Java

Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.

## <a name="unsupported-clients"></a>Client non supportati

I client seguenti sono disponibili ma sono sperimentali o non ufficiali. Essi non sono attualmente supportati e non può mai essere.

* [Client C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
