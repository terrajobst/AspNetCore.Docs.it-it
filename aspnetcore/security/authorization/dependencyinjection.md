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
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="31914-103">Inserimento di dipendenze nei gestori di requisito</span><span class="sxs-lookup"><span data-stu-id="31914-103">Dependency Injection in requirement handlers</span></span>

<a name=security-authorization-di></a>

<span data-ttu-id="31914-104">[I gestori di autorizzazione devono essere registrati](policies.md#security-authorization-policies-based-handler-registration) nella raccolta durante la configurazione del servizio (con [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="31914-104">[Authorization handlers must be registered](policies.md#security-authorization-policies-based-handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="31914-105">Si supponga un repository di regole che si desidera valutare all'interno di un gestore di autorizzazione e il repository è stato registrato nella raccolta di servizio.</span><span class="sxs-lookup"><span data-stu-id="31914-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span>  <span data-ttu-id="31914-106">Autorizzazione risolveranno e inserire che nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="31914-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="31914-107">Ad esempio, se si desidera utilizzare le pagine ASP. Registrazione dell'infrastruttura che si desidera inserire NET `ILoggerFactory` nel gestore.</span><span class="sxs-lookup"><span data-stu-id="31914-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="31914-108">Un gestore di questo tipo potrebbe essere simile:</span><span class="sxs-lookup"><span data-stu-id="31914-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="31914-109">È necessario registrare il gestore con `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="31914-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
   ```

<span data-ttu-id="31914-110">Un'istanza dell'oggetto gestore essere creata all'avvio dell'applicazione e verrà eseguito DI inserire registrato `ILoggerFactory` del costruttore.</span><span class="sxs-lookup"><span data-stu-id="31914-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="31914-111">I gestori che utilizzano Entity Framework non devono essere registrati come singleton.</span><span class="sxs-lookup"><span data-stu-id="31914-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
