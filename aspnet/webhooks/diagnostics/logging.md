---
uid: webhooks/diagnostics/logging
title: ASP.NET Webhook registrazione | Documenti Microsoft
author: rick-anderson
description: Come eseguire la registrazione di ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET Webhook registrazione

Microsoft ASP.NET WebHooks Usa la registrazione come modalità di segnalazione di problemi e problemi. Per impostazione predefinita, i log vengono scritti utilizzando [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) in cui possono essere gestite tramite [listener di traccia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) come qualsiasi altro flusso di log.

Quando si distribuisce l'applicazione Web come un'App Web di Azure, i log vengono rilevati automaticamente e possono essere gestiti insieme a qualsiasi altro [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) registrazione. Per informazioni dettagliate, vedere [abilitare la registrazione diagnostica per App web nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Inoltre, i registri possono essere ottenuti direttamente all'interno di Visual Studio, come descritto in [risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Reindirizzamento di registri

Invece di scrivere i log per [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), è possibile fornire un'implementazione alternativa per il log che è possibile accedere direttamente a un responsabile di log, ad esempio [Log4Net](http://logging.apache.org/log4net/) e [NLog ](http://nlog-project.org/). È sufficiente fornire un'implementazione di [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrarlo con un motore di aggiunta della dipendenza di propria scelta e sarà prelevata da Microsoft ASP.NET WebHooks. Vedere [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) per informazioni dettagliate.
