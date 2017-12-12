---
title: Autorizzazione semplice
author: rick-anderson
description: Questo documento viene illustrato come utilizzare l'attributo Authorize per limitare l'accesso alle azioni e i controller di ASP.NET Core.
keywords: ASP.NET Core, autorizzazione, AuthorizeAttribute
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="d2b30-104">Autorizzazione semplice</span><span class="sxs-lookup"><span data-stu-id="d2b30-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="d2b30-105">L'autorizzazione in MVC è controllata tramite il `AuthorizeAttribute` attributo e i relativi parametri diversi.</span><span class="sxs-lookup"><span data-stu-id="d2b30-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="d2b30-106">Nella forma più semplice, l'applicazione di `AuthorizeAttribute` dell'attributo a un controller o un'azione verrà limitato l'accesso al controller o un'azione da qualsiasi utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="d2b30-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="d2b30-107">Ad esempio, il codice seguente limita l'accesso per il `AccountController` da qualsiasi utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="d2b30-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="d2b30-108">Se si desidera applicare l'autorizzazione a un'azione anziché il controller, applicare il `AuthorizeAttribute` attributo per l'azione di se stesso:</span><span class="sxs-lookup"><span data-stu-id="d2b30-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="d2b30-109">Ora possono accedere solo gli utenti autenticati di `Logout` (funzione).</span><span class="sxs-lookup"><span data-stu-id="d2b30-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="d2b30-110">È inoltre possibile utilizzare il `AllowAnonymousAttribute` attributo per consentire l'accesso agli utenti non autenticati di singole azioni.</span><span class="sxs-lookup"><span data-stu-id="d2b30-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="d2b30-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d2b30-111">For example:</span></span>

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

<span data-ttu-id="d2b30-112">In questo modo solo gli utenti autenticati per il `AccountController`, fatta eccezione per il `Login` azione, che è accessibile da tutti gli utenti, indipendentemente dal loro stato autenticato o anonimo / non autenticato.</span><span class="sxs-lookup"><span data-stu-id="d2b30-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="d2b30-113">`[AllowAnonymous]`Ignora tutte le istruzioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="d2b30-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="d2b30-114">Se si applica a combinare `[AllowAnonymous]` e qualsiasi `[Authorize]` attributo quindi gli attributi di autorizzazione verranno sempre ignorati.</span><span class="sxs-lookup"><span data-stu-id="d2b30-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="d2b30-115">Ad esempio, se si applica `[AllowAnonymous]` il controller a livello qualsiasi `[Authorize]` gli attributi allo stesso controller o in qualsiasi azione all'interno di esso verranno ignorati.</span><span class="sxs-lookup"><span data-stu-id="d2b30-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
