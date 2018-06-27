---
title: Autorizzazione semplice in ASP.NET Core
author: rick-anderson
description: Imparare a usare l'attributo Authorize per limitare l'accesso alle azioni e controller di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961123"
---
# <a name="simple-authorization-in-aspnet-core"></a>Autorizzazione semplice in ASP.NET Core

<a name="security-authorization-simple"></a>

L'autorizzazione in MVC è controllata tramite il `AuthorizeAttribute` attributo e i relativi parametri diversi. Nella sua forma più semplice, l'applicazione di `AuthorizeAttribute` attributo a un controller o un'azione verrà limitato l'accesso al controller o azione da qualsiasi utente autenticato.

Ad esempio, il codice seguente consente di limitare l'accesso al `AccountController` da qualsiasi utente autenticato.

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Se si desidera applicare l'autorizzazione a un'azione anziché il controller, applicare il `AuthorizeAttribute` attributo per l'azione di se stesso:

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

Ora possono accedere solo gli utenti autenticati il `Logout` (funzione).

È anche possibile usare il `AllowAnonymous` attributo per consentire l'accesso da utenti non autenticati per le singole azioni. Ad esempio:

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

In questo modo solo gli utenti autenticati per il `AccountController`, ad eccezione del `Login` azione, che è accessibile da parte degli utenti, indipendentemente dal loro stato autenticato o anonimo / non autenticato.

> [!WARNING]
> `[AllowAnonymous]` Ignora tutte le istruzioni di autorizzazione. Se si combinano `[AllowAnonymous]` e di qualsiasi `[Authorize]` attributo, il `[Authorize]` gli attributi vengono ignorati. Ad esempio se si applica `[AllowAnonymous]` a livello di controller, qualsiasi `[Authorize]` attributi allo stesso controller (o su qualsiasi azione all'interno di esso) viene ignorato.
