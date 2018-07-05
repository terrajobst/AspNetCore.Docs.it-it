---
uid: webhooks/diagnostics/logging
title: Webhook ASP.NET la registrazione | Microsoft Docs
author: rick-anderson
description: Come eseguire la registrazione in ASP.NET i Webhook.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 65e4d49474034406be835eb31378c81ba0706da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828455"
---
# <a name="aspnet-webhooks-logging"></a>Webhook ASP.NET la registrazione

Microsoft ASP.NET WebHooks Usa la registrazione come modalità di segnalazione dei problemi. Per impostazione predefinita, i log vengono scritti utilizzando [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) in cui possono essere gestiti utilizzando [listener di traccia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) come qualsiasi altro flusso di log.

Quando si distribuisce l'applicazione Web come App Web di Azure, i log vengono automaticamente rilevati e possono essere gestiti insieme a qualsiasi altra [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) la registrazione. Per informazioni dettagliate, vedere [abilitare la registrazione diagnostica per le app web nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Inoltre, possono essere ottenuti i log direttamente da Visual Studio come descritto in [risolvere i problemi di un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Reindirizzamento dei log

Invece di scrivere i log per [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), è possibile fornire un'implementazione alternativa per il log che possa accedere direttamente a una gestione log, ad esempio [Log4Net](http://logging.apache.org/log4net/) e [NLog ](http://nlog-project.org/). È sufficiente fornire un'implementazione di [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrarlo con un motore di inserimento delle dipendenze di propria scelta e sarà prelevata da WebHooks ASP.NET Microsoft. Vedi [inserimento delle dipendenze in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) per informazioni dettagliate.
