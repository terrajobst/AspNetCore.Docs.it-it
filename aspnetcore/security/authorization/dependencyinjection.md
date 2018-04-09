---
title: Inserimento di dipendenze nei gestori requisito in ASP.NET Core
author: rick-anderson
description: Informazioni su come inserire i gestori di requisiti di autorizzazione in un'app di ASP.NET Core mediante l'inserimento di dipendenza.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 4de7f0e49ade459968f8c30fbad76ce96a65815f
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Inserimento di dipendenze nei gestori requisito in ASP.NET Core

<a name="security-authorization-di"></a>

[I gestori di autorizzazione devono essere registrati](xref:security/authorization/policies#handler-registration) nella raccolta durante la configurazione del servizio (utilizzando [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

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
