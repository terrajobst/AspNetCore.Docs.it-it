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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dipendenze del ricevitore Webhook ASP.NET

Microsoft ASP.NET WebHooks è progettato con l'inserimento di dipendenza presente. La maggior parte delle dipendenze nel sistema possono essere sostituite con implementazioni alternative usando un motore di inserimento delle dipendenze.

Vedi [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) per un elenco delle dipendenze del ricevitore. Se non è stata registrata alcuna dipendenza, viene utilizzata un'implementazione predefinita. Vedi [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) per un elenco delle implementazioni predefinite.
