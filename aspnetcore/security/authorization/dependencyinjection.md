---
title: Inserimento di dipendenze nei gestori di requisiti in ASP.NET Core
author: rick-anderson
description: Informazioni su come inserire i gestori di requisiti di autorizzazione in un'app ASP.NET Core con inserimento delle dipendenze.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342114"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Inserimento di dipendenze nei gestori di requisiti in ASP.NET Core

<a name="security-authorization-di"></a>

[I gestori di autorizzazione devono essere registrati](xref:security/authorization/policies#handler-registration) nella raccolta durante la configurazione del servizio (usando [inserimento delle dipendenze](xref:fundamentals/dependency-injection)).

Si supponga un repository di regole che si desidera valutare all'interno di un gestore di autorizzazione e tale repository è stato registrato nell'insieme del servizio. Autorizzazione risolveranno e inserirlo nel costruttore.

Ad esempio, se si desidera utilizzare le pagine ASP. Registrazione dell'infrastruttura è preferibile inserire NET `ILoggerFactory` nei gestore. Un gestore di questo tipo potrebbe essere simile:

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

Un'istanza del gestore verrà essere creata all'avvio dell'applicazione e l'inserimento delle dipendenze verrà inserire registrato `ILoggerFactory` il costruttore.

> [!NOTE]
> I gestori che usano Entity Framework non devono essere registrati come singleton.
