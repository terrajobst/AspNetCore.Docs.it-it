---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web ASP.NET Core e sull'host generico .NET, responsabili della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276617"
---
# <a name="host-in-aspnet-core"></a>Hosting in ASP.NET Core

Le app .NET configurano e avviano un *host*. L'host è responsabile della gestione dell'avvio e della durata delle app. Sono disponibili per l'uso due host API:

* [Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.
* [Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background. In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web. L'host generico è destinato a sostituire l'host Web.

Per l'hosting delle *app Web* ASP.NET Core, gli sviluppatori devono usare l'host Web basato su [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Per l'hosting delle app *non Web*, gli sviluppatori devono usare l'host generico basato su [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
