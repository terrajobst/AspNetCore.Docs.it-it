---
title: Piattaforme supportate da ASP.NET Core SignalR
author: bradygaster
description: Informazioni sulle piattaforme supportate per ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 054965921c87c1a9be27e5ddaa8a87b0fa1f4113
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78668140"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Piattaforme supportate da ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Requisiti di sistema del server di

SignalR per ASP.NET Core supporta qualsiasi piattaforma server supportata da ASP.NET Core.

## <a name="javascript-client"></a>Client JavaScript

Il [client JavaScript](xref:signalr/javascript-client) viene eseguito in NodeJS 8 e versioni successive e nei browser seguenti:

| Browser                         | Versione         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | &dagger; corrente |
| Mozilla Firefox                 | &dagger; corrente |
| Google Chrome; include Android | &dagger; corrente |
| Safari include iOS            | &dagger; corrente |
| Microsoft Internet Explorer     | 11              |

&dagger;*corrente* si riferisce alla versione più recente del browser.

## <a name="net-client"></a>Client .NET

Il [client .NET](xref:signalr/dotnet-client) viene eseguito su qualsiasi piattaforma supportata da ASP.NET Core. Ad esempio, [gli sviluppatori Novell possono usare SignalR](https://github.com/aspnet/Announcements/issues/305) per creare app Android usando Novell. Android 8.4.0.1 e versioni successive e app iOS usando Novell. iOS 11.14.0.4 e versioni successive.

Se il server esegue IIS, il trasporto WebSocket richiede IIS 8,0 o versioni successive in Windows Server 2012 o versioni successive. Gli altri trasporti sono supportati in tutte le piattaforme.

## <a name="java-client"></a>Client Java

Il [client Java](xref:signalr/java-client) supporta Java 8 e versioni successive.

## <a name="unsupported-clients"></a>Client non supportati

I client seguenti sono disponibili, ma sono sperimentali o non ufficiali. Non sono attualmente supportati e potrebbero non essere mai.

* [C++client](https://github.com/aspnet/SignalR-Client-Cpp)

* [Client Swift](https://github.com/moozzyk/SignalR-Client-Swift)
