---
title: Autorizzazione semplice in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare l'attributo Authorize per limitare l'accesso alle azioni e controller ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897678"
---
# <a name="simple-authorization-in-aspnet-core"></a>Autorizzazione semplice in ASP.NET Core

<a name="security-authorization-simple"></a>

Autorizzazione in MVC è controllata tramite il `AuthorizeAttribute` attributo e i relativi parametri diversi. Nella forma più semplice, applicare il `AuthorizeAttribute` attributo a un controller o azione verrà limitato l'accesso al controller o azione per tutti gli utenti autenticati.

Ad esempio, il codice riportato di seguito limitano l'accesso al `AccountController` da qualsiasi utente autenticato.

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

Se si desidera applicare l'autorizzazione a un'azione piuttosto che il controller, applicare il `AuthorizeAttribute` dell'attributo dell'azione stessa:

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

È ora possono accedere solo agli utenti autenticati di `Logout` (funzione).

È anche possibile usare il `AllowAnonymous` attributo per consentire l'accesso gli utenti non autenticati a singole azioni. Ad esempio:

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

In questo modo solo gli utenti autenticati per il `AccountController`, tranne che per il `Login` azione, che è accessibile da parte degli utenti, indipendentemente dal loro stato autenticato o non autenticato o anonimo.

> [!WARNING]
> `[AllowAnonymous]` Consente di ignorare tutte le istruzioni di autorizzazione. Se si combinano `[AllowAnonymous]` e l'eventuale `[Authorize]` attributo, il `[Authorize]` gli attributi vengono ignorati. Ad esempio se si applicano `[AllowAnonymous]` a livello di controller, qualsiasi `[Authorize]` attributi allo stesso controller (o in qualsiasi azione all'interno di esso) viene ignorato.
