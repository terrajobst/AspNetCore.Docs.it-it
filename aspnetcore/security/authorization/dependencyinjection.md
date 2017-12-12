---
title: Inserimento di dipendenze nei gestori di requisito
author: rick-anderson
description: Questo documento viene descritto come inserire i gestori di requisiti di autorizzazione in un'applicazione ASP.NET di base mediante l'inserimento di dipendenza.
keywords: ASP.NET di base, l'inserimento di dipendenze, il gestore di autorizzazione
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a>Inserimento di dipendenze nei gestori di requisito

<a name="security-authorization-di"></a>

[I gestori di autorizzazione devono essere registrati](policies.md#handler-registration) nella raccolta durante la configurazione del servizio (con [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

Si supponga un repository di regole che si desidera valutare all'interno di un gestore di autorizzazione e il repository è stato registrato nella raccolta di servizio. Autorizzazione risolveranno e inserire che nel costruttore.

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
