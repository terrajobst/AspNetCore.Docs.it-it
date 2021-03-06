---
title: Inserimento delle dipendenze nei gestori di requisiti in ASP.NET Core
author: rick-anderson
description: Informazioni su come inserire i gestori dei requisiti di autorizzazione in un'app ASP.NET Core usando l'inserimento di dipendenze.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666089"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Inserimento delle dipendenze nei gestori di requisiti in ASP.NET Core

<a name="security-authorization-di"></a>

I [gestori di autorizzazione devono essere registrati](xref:security/authorization/policies#handler-registration) nella raccolta di servizi durante la configurazione (usando l' [inserimento di dipendenze](xref:fundamentals/dependency-injection)).

Si supponga di avere un repository di regole che si desidera valutare all'interno di un gestore autorizzazioni e che il repository sia stato registrato nella raccolta di servizi. L'autorizzazione lo risolve e inserisce nel costruttore.

Ad esempio, se si desidera utilizzare ASP. Infrastruttura di registrazione di NET è necessario inserire `ILoggerFactory` nel gestore. Questo gestore potrebbe essere simile al seguente:

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

Registrare il gestore con `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Quando l'applicazione viene avviata, verrà creata un'istanza del gestore che inserisce il `ILoggerFactory` registrato nel costruttore.

> [!NOTE]
> I gestori che usano Entity Framework non devono essere registrati come singleton.
