---
title: Inserimento di dipendenze nei gestori di requisito
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 37d197d7696a6e91fa236b2defc577959c95c49f
ms.sourcegitcommit: 0a70706a3814d2684f3ff96095d1e8291d559cc7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a>Inserimento di dipendenze nei gestori di requisito

<a name=security-authorization-di></a>

[I gestori di autorizzazione devono essere registrati](policies.md#security-authorization-policies-based-handler-registration) nella raccolta durante la configurazione del servizio (con [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

Si supponga un repository di regole che si desidera valutare all'interno di un gestore di autorizzazione e il repository è stato registrato nella raccolta di servizio.  Autorizzazione risolveranno e inserire che nel costruttore.

Ad esempio, se si desidera utilizzare le pagine ASP. Registrazione dell'infrastruttura che si desidera inserire NET `ILoggerFactory` nel gestore. Un gestore di questo tipo potrebbe essere simile:

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

È necessario registrare il gestore con `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
   ```

Un'istanza dell'oggetto gestore essere creata all'avvio dell'applicazione e verrà eseguito DI inserire registrato `ILoggerFactory` del costruttore.

> [!NOTE]
> I gestori che utilizzano Entity Framework non devono essere registrati come singleton.
