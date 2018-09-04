---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web ASP.NET Core e sull'host generico .NET, responsabili della gestione dell'avvio e della durata delle app.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336050"
---
# <a name="host-in-aspnet-core"></a>Hosting in ASP.NET Core

Le app .NET configurano e avviano un *host*. L'host è responsabile della gestione dell'avvio e della durata delle app. Sono disponibili per l'uso due host API:

* [Host Web](xref:fundamentals/host/web-host) &ndash; Adatto per l'hosting di app Web.
* [Host generico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 o versioni successive) &ndash; Adatto per l'hosting di app non Web, ad esempio app che eseguono attività in background. In una versione futura, l'host generico sarà adatto per l'hosting di qualsiasi tipo di app, incluse le app Web. L'host generico è destinato a sostituire l'host Web.

Per l'hosting delle *app Web* ASP.NET Core, gli sviluppatori devono usare l'host Web basato su <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Per l'hosting delle *app non Web*, gli sviluppatori devono usare l'host generico basato su <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Informazioni su come migliorare un'app ASP.NET Core da un assembly con o senza riferimenti usando un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.
