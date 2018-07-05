---
uid: webhooks/receiving/dependencies
title: Dipendenze del ricevitore Webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Dipendenze del ricevitore e inserimento delle dipendenze in ASP.NET i Webhook.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817837"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="6a283-103">Dipendenze del ricevitore Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6a283-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="6a283-104">Microsoft ASP.NET WebHooks è progettato con l'inserimento di dipendenza presente.</span><span class="sxs-lookup"><span data-stu-id="6a283-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="6a283-105">La maggior parte delle dipendenze nel sistema possono essere sostituite con implementazioni alternative usando un motore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6a283-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="6a283-106">Vedi [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze del ricevitore.</span><span class="sxs-lookup"><span data-stu-id="6a283-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="6a283-107">Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6a283-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="6a283-108">Vedi [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="6a283-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
