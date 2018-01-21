---
title: Autorizzazione semplice
author: rick-anderson
description: Questo documento viene illustrato come utilizzare l'attributo Authorize per limitare l'accesso alle azioni e i controller di ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f1d5671785da815f2f4fcf5bef1352f4c9e62877
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="simple-authorization"></a>Autorizzazione semplice

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

È inoltre possibile utilizzare il `AllowAnonymousAttribute` attributo per consentire l'accesso agli utenti non autenticati di singole azioni. Ad esempio:

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
> `[AllowAnonymous]`Ignora tutte le istruzioni di autorizzazione. Se si applica a combinare `[AllowAnonymous]` e qualsiasi `[Authorize]` attributo quindi gli attributi di autorizzazione verranno sempre ignorati. Ad esempio, se si applica `[AllowAnonymous]` il controller a livello qualsiasi `[Authorize]` gli attributi allo stesso controller o in qualsiasi azione all'interno di esso verranno ignorati.
