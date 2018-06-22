---
title: Autorizzazione semplice in ASP.NET Core
author: rick-anderson
description: Imparare a usare l'attributo Authorize per limitare l'accesso alle azioni e controller di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272065"
---
# <a name="simple-authorization-in-aspnet-core"></a>Autorizzazione semplice in ASP.NET Core

<a name="security-authorization-simple"></a>

L'autorizzazione in MVC è controllata tramite il `AuthorizeAttribute` attributo e i relativi parametri diversi. Nella forma più semplice, l'applicazione di `AuthorizeAttribute` dell'attributo a un controller o un'azione verrà limitato l'accesso al controller o un'azione da qualsiasi utente autenticato.

Ad esempio, il codice seguente limita l'accesso per il `AccountController` da qualsiasi utente autenticato.

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

Ora possono accedere solo gli utenti autenticati di `Logout` (funzione).

È inoltre possibile utilizzare il `AllowAnonymous` attributo per consentire l'accesso agli utenti non autenticati di singole azioni. Ad esempio:

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

In questo modo solo gli utenti autenticati per il `AccountController`, fatta eccezione per il `Login` azione, che è accessibile da tutti gli utenti, indipendentemente dal loro stato autenticato o anonimo / non autenticato.

>[!WARNING]
> `[AllowAnonymous]` Ignora tutte le istruzioni di autorizzazione. Se si applica a combinare `[AllowAnonymous]` e qualsiasi `[Authorize]` attributo quindi gli attributi di autorizzazione verranno sempre ignorati. Ad esempio, se si applica `[AllowAnonymous]` il controller a livello qualsiasi `[Authorize]` gli attributi allo stesso controller o in qualsiasi azione all'interno di esso verranno ignorati.
