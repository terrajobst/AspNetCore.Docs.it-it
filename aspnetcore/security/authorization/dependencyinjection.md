---
title: Inserimento di dipendenze nei gestori requisito in ASP.NET Core
author: rick-anderson
description: Informazioni su come inserire i gestori di requisiti di autorizzazione in un'app di ASP.NET Core mediante l'inserimento di dipendenza.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273722"
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
