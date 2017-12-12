---
uid: webhooks/receiving/dependencies
title: Dipendenze di ricevitore ASP.NET Webhook | Documenti Microsoft
author: rick-anderson
description: Dipendenze di ricevitore e inserimento di dipendenze in ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="4da43-103">Dipendenze di ricevitore Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4da43-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="4da43-104">Microsoft ASP.NET WebHooks è progettata con l'inserimento di dipendenze in considerazione.</span><span class="sxs-lookup"><span data-stu-id="4da43-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="4da43-105">La maggior parte delle dipendenze del sistema possono essere sostituite con implementazioni alternative usando un motore di aggiunta della dipendenza.</span><span class="sxs-lookup"><span data-stu-id="4da43-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="4da43-106">Vedere [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze di ricevitore.</span><span class="sxs-lookup"><span data-stu-id="4da43-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="4da43-107">Se non è stata registrata alcuna dipendenza, un'implementazione predefinita viene utilizzata.</span><span class="sxs-lookup"><span data-stu-id="4da43-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="4da43-108">Vedere [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="4da43-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
