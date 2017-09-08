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
ms.openlocfilehash: 013ce0d9ac1e9c1b6bb541b9fa66218c3fd799bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="simple-authorization"></a>Autorizzazione semplice

<a name=security-authorization-simple></a>

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
