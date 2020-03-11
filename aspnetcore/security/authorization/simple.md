---
title: Autorizzazione semplice in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare l'attributo di autorizzazione per limitare l'accesso a controller e azioni ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663583"
---
# <a name="simple-authorization-in-aspnet-core"></a>Autorizzazione semplice in ASP.NET Core

<a name="security-authorization-simple"></a>

L'autorizzazione in MVC viene controllata tramite l'attributo `AuthorizeAttribute` e i vari parametri. Al livello più semplice, l'applicazione dell'attributo `AuthorizeAttribute` a un controller o a un'azione limita l'accesso al controller o all'azione a qualsiasi utente autenticato.

Il codice seguente, ad esempio, limita l'accesso al `AccountController` a qualsiasi utente autenticato.

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

Se si desidera applicare l'autorizzazione a un'azione anziché al controller, applicare l'attributo `AuthorizeAttribute` all'azione stessa:

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

A questo punto, solo gli utenti autenticati possono accedere alla funzione `Logout`.

È anche possibile usare l'attributo `AllowAnonymous` per consentire l'accesso da parte di utenti non autenticati a singole azioni. Ad esempio:

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

Ciò consente solo agli utenti autenticati di `AccountController`, ad eccezione dell'azione `Login`, che è accessibile a tutti, indipendentemente dallo stato autenticato o non autenticato/anonimo.

> [!WARNING]
> `[AllowAnonymous]` ignora tutte le istruzioni di autorizzazione. Se si combinano `[AllowAnonymous]` e qualsiasi attributo `[Authorize]`, gli attributi `[Authorize]` vengono ignorati. Se ad esempio si applicano `[AllowAnonymous]` a livello di controller, tutti gli attributi di `[Authorize]` sullo stesso controller (o su qualsiasi azione al suo interno) vengono ignorati.
