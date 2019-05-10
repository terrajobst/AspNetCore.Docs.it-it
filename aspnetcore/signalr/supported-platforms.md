---
title: Piattaforme supportate di ASP.NET Core SignalR
author: bradygaster
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892018"
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

Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma supportata da ASP.NET Core. Ad esempio, [gli sviluppatori di Xamarin possono usare SignalR](https://github.com/aspnet/Announcements/issues/305) per la creazione di App per Android usando xamarin. Android 8.4.0.1 e versioni successive e le app iOS con xamarin. IOS 11.14.0.4 e versioni successive.

Se il server è in esecuzione IIS, il trasporto WebSocket richiede IIS 8.0 o versione successiva in Windows Server 2012 o versioni successive. Sono supportati altri tipi di trasporto in tutte le piattaforme.

## <a name="java-client"></a>Client Java

Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.

## <a name="unsupported-clients"></a>Client non supportati

I client seguenti sono disponibili ma sono sperimentali o non ufficiali. Essi non sono attualmente supportati e non può mai essere.

* [Client C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
