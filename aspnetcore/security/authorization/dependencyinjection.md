---
title: Inserimento di dipendenze nei gestori di requisiti in ASP.NET Core
author: rick-anderson
description: Informazioni su come inserire i gestori di requisiti di autorizzazione in un'app ASP.NET Core con inserimento delle dipendenze.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896368"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="ddacb-103">Inserimento di dipendenze nei gestori di requisiti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddacb-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="ddacb-104">[I gestori di autorizzazione devono essere registrati](xref:security/authorization/policies#handler-registration) nella raccolta durante la configurazione del servizio (usando [inserimento delle dipendenze](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="ddacb-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="ddacb-105">Si supponga un repository di regole che si desidera valutare all'interno di un gestore di autorizzazione e tale repository è stato registrato nell'insieme del servizio.</span><span class="sxs-lookup"><span data-stu-id="ddacb-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="ddacb-106">Autorizzazione risolveranno e inserirlo nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="ddacb-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="ddacb-107">Ad esempio, se si desidera utilizzare le pagine ASP. Registrazione dell'infrastruttura è preferibile inserire NET `ILoggerFactory` nei gestore.</span><span class="sxs-lookup"><span data-stu-id="ddacb-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="ddacb-108">Un gestore di questo tipo potrebbe essere simile:</span><span class="sxs-lookup"><span data-stu-id="ddacb-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="ddacb-109">È necessario registrare il gestore con `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="ddacb-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="ddacb-110">Un'istanza del gestore verrà essere creata all'avvio dell'applicazione e l'inserimento delle dipendenze verrà inserire registrato `ILoggerFactory` il costruttore.</span><span class="sxs-lookup"><span data-stu-id="ddacb-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="ddacb-111">I gestori che usano Entity Framework non devono essere registrati come singleton.</span><span class="sxs-lookup"><span data-stu-id="ddacb-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
