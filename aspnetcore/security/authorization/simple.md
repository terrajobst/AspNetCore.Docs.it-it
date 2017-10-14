---
title: Autorizzazione semplice
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 91f402b1cbf73e212418d197a8a7230ce22e9e1d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="simple-authorization"></a>Autorizzazione semplice

<a name="security-authorization-simple"></a>

L'autorizzazione in MVC è controllata tramite il `AuthorizeAttribute` attributo e i relativi parametri diversi. Il più semplice applicazione di `AuthorizeAttribute` dell'attributo a un controller o un'azione verrà limitato l'accesso al controller o un'azione da qualsiasi utente autenticato.

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

Se si desidera applicare l'autorizzazione a un'azione anziché il controller è sufficiente applicare il `AuthorizeAttribute` attributo per l'azione.

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

Solo gli utenti autenticati potranno ora accedere la funzione di disconnessione.

È inoltre possibile utilizzare il `AllowAnonymousAttribute` attributo per consentire l'accesso agli utenti non autenticati di singole azioni; ad esempio

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
