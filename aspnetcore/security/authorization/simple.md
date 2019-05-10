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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="1fb40-103">Autorizzazione semplice in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1fb40-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="1fb40-104">Autorizzazione in MVC è controllata tramite il `AuthorizeAttribute` attributo e i relativi parametri diversi.</span><span class="sxs-lookup"><span data-stu-id="1fb40-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="1fb40-105">Nella forma più semplice, applicare il `AuthorizeAttribute` attributo a un controller o azione verrà limitato l'accesso al controller o azione per tutti gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="1fb40-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="1fb40-106">Ad esempio, il codice riportato di seguito limitano l'accesso al `AccountController` da qualsiasi utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="1fb40-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="1fb40-107">Se si desidera applicare l'autorizzazione a un'azione piuttosto che il controller, applicare il `AuthorizeAttribute` dell'attributo dell'azione stessa:</span><span class="sxs-lookup"><span data-stu-id="1fb40-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="1fb40-108">È ora possono accedere solo agli utenti autenticati di `Logout` (funzione).</span><span class="sxs-lookup"><span data-stu-id="1fb40-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="1fb40-109">È anche possibile usare il `AllowAnonymous` attributo per consentire l'accesso gli utenti non autenticati a singole azioni.</span><span class="sxs-lookup"><span data-stu-id="1fb40-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="1fb40-110">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1fb40-110">For example:</span></span>

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

<span data-ttu-id="1fb40-111">In questo modo solo gli utenti autenticati per il `AccountController`, tranne che per il `Login` azione, che è accessibile da parte degli utenti, indipendentemente dal loro stato autenticato o non autenticato o anonimo.</span><span class="sxs-lookup"><span data-stu-id="1fb40-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="1fb40-112">`[AllowAnonymous]` Consente di ignorare tutte le istruzioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="1fb40-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="1fb40-113">Se si combinano `[AllowAnonymous]` e l'eventuale `[Authorize]` attributo, il `[Authorize]` gli attributi vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="1fb40-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="1fb40-114">Ad esempio se si applicano `[AllowAnonymous]` a livello di controller, qualsiasi `[Authorize]` attributi allo stesso controller (o in qualsiasi azione all'interno di esso) viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="1fb40-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
