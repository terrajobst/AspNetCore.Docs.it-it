---
title: Piattaforme supportate da ASP.NET Core SignalR
author: bradygaster
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426978"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Piattaforme supportate da ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Requisiti di sistema del server

SignalR per ASP.NET Core supporta qualsiasi piattaforma server supportata da ASP.NET Core.

## <a name="javascript-client"></a>Client JavaScript

Il [client JavaScript](https://www.npmjs.com/package/@aspnet/signalr) viene eseguito in NodeJS 8 e versioni successive e nei browser seguenti:

| Browser                         | Versione         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | &dagger; corrente |
| Mozilla Firefox                 | &dagger; corrente |
| Google Chrome; include Android | &dagger; corrente |
| Safari include iOS            | &dagger; corrente |
| Microsoft Internet Explorer     | 11              |

&dagger;*corrente* si riferisce alla versione più recente del browser.

## <a name="net-client"></a>Client .NET

Il [client .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) viene eseguito su qualsiasi piattaforma supportata da ASP.NET Core. Ad esempio, [gli sviluppatori Novell possono usare SignalR](https://github.com/aspnet/Announcements/issues/305) per la creazione di app Android usando Novell. Android 8.4.0.1 e versioni successive e app iOS con Novell. iOS 11.14.0.4 e versioni successive.

Se il server esegue IIS, il trasporto WebSocket richiede IIS 8,0 o versioni successive in Windows Server 2012 o versioni successive. Gli altri trasporti sono supportati in tutte le piattaforme.

## <a name="java-client"></a>Client Java

Il [client Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supporta Java 8 e versioni successive.

## <a name="unsupported-clients"></a>Client non supportati

I client seguenti sono disponibili, ma sono sperimentali o non ufficiali. Non sono attualmente supportati e potrebbero non essere mai.

* [C++client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Client Swift](https://github.com/moozzyk/SignalR-Client-Swift)
