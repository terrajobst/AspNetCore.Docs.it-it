---
uid: webhooks/receiving/dependencies
title: Dipendenze del ricevitore Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Dipendenze del ricevitore e inserimento delle dipendenze in ASP.NET i Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401185"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="e61ca-103">Dipendenze del ricevitore Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e61ca-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="e61ca-104">Microsoft ASP.NET WebHooks è progettato con l'inserimento di dipendenza presente.</span><span class="sxs-lookup"><span data-stu-id="e61ca-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="e61ca-105">La maggior parte delle dipendenze nel sistema possono essere sostituite con implementazioni alternative usando un motore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e61ca-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="e61ca-106">Vedi [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze del ricevitore.</span><span class="sxs-lookup"><span data-stu-id="e61ca-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="e61ca-107">Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e61ca-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="e61ca-108">Vedi [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="e61ca-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
