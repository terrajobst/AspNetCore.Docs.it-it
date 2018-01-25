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
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="6889e-103">ASP.NET Webhook registrazione</span><span class="sxs-lookup"><span data-stu-id="6889e-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="6889e-104">Microsoft ASP.NET WebHooks Usa la registrazione come modalità di segnalazione di problemi e problemi.</span><span class="sxs-lookup"><span data-stu-id="6889e-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="6889e-105">Per impostazione predefinita, i log vengono scritti utilizzando [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) in cui possono essere gestite tramite [listener di traccia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) come qualsiasi altro flusso di log.</span><span class="sxs-lookup"><span data-stu-id="6889e-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="6889e-106">Quando si distribuisce l'applicazione Web come un'App Web di Azure, i log vengono rilevati automaticamente e possono essere gestiti insieme a qualsiasi altro [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) registrazione.</span><span class="sxs-lookup"><span data-stu-id="6889e-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="6889e-107">Per informazioni dettagliate, vedere [abilitare la registrazione diagnostica per App web nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="6889e-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="6889e-108">Inoltre, i registri possono essere ottenuti direttamente all'interno di Visual Studio, come descritto in [risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="6889e-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="6889e-109">Reindirizzamento di registri</span><span class="sxs-lookup"><span data-stu-id="6889e-109">Redirecting Logs</span></span>

<span data-ttu-id="6889e-110">Invece di scrivere i log per [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), è possibile fornire un'implementazione alternativa per il log che è possibile accedere direttamente a un responsabile di log, ad esempio [Log4Net](http://logging.apache.org/log4net/) e [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="6889e-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="6889e-111">È sufficiente fornire un'implementazione di [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrarlo con un motore di aggiunta della dipendenza di propria scelta e sarà prelevata da Microsoft ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="6889e-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="6889e-112">Vedere [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="6889e-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
