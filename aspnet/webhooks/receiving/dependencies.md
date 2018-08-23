---
uid: webhooks/receiving/dependencies
title: Dipendenze del ricevitore Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Dipendenze del ricevitore e inserimento delle dipendenze in ASP.NET i Webhook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833308"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="5d05f-103">Dipendenze del ricevitore Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5d05f-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="5d05f-104">Microsoft ASP.NET WebHooks è progettato con l'inserimento di dipendenza presente.</span><span class="sxs-lookup"><span data-stu-id="5d05f-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="5d05f-105">La maggior parte delle dipendenze nel sistema possono essere sostituite con implementazioni alternative usando un motore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="5d05f-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="5d05f-106">Vedi [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze del ricevitore.</span><span class="sxs-lookup"><span data-stu-id="5d05f-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="5d05f-107">Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5d05f-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="5d05f-108">Vedi [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="5d05f-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
