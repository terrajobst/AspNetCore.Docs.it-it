---
title: Autorizzazione basata sui ruoli
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: d8dfcbb16ee7977d197b019c4e5e1b30fff17755
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="cc9f9-103">Autorizzazione basata sui ruoli</span><span class="sxs-lookup"><span data-stu-id="cc9f9-103">Role based Authorization</span></span>

<a name=security-authorization-role-based></a>

<span data-ttu-id="cc9f9-104">Quando viene creata un'identità può appartenere a uno o più ruoli, ad esempio Tracy può appartenere ai ruoli di amministratore e utente pur Scott può appartenere solo al ruolo utente.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-104">When an identity is created it may belong to one or more roles, for example Tracy may belong to the Administrator and User roles whilst Scott may only belong to the user role.</span></span> <span data-ttu-id="cc9f9-105">Come questi ruoli vengono creati e gestiti dipende dall'archivio di backup del processo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-105">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="cc9f9-106">I ruoli vengono esposte allo sviluppatore di tramite il [IsInRole](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal.isinrole(v=vs.110).aspx) proprietà il [ClaimsPrincipal](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal(v=vs.110).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-106">Roles are exposed to the developer through the [IsInRole](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal.isinrole(v=vs.110).aspx) property on the [ClaimsPrincipal](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal(v=vs.110).aspx) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="cc9f9-107">Aggiunta di controlli del ruolo</span><span class="sxs-lookup"><span data-stu-id="cc9f9-107">Adding role checks</span></span>

<span data-ttu-id="cc9f9-108">Controlli di autorizzazione basata sui ruoli sono dichiarativi, lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, specificare i ruoli che l'utente corrente deve essere un membro di accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-108">Role based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="cc9f9-109">Ad esempio il codice seguente sarebbe limitare l'accesso a tutte le azioni nel `AdministrationController` agli utenti che sono membri del `Administrator` gruppo.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-109">For example the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="cc9f9-110">È possibile specificare più ruoli come un elenco separato da virgole.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-110">You can specify multiple roles as a comma separated list;</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="cc9f9-111">Questo controller possono essere solo accessibile dagli utenti che sono membri del `HRManager` ruolo o `Finance` ruolo.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-111">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="cc9f9-112">Se si applicano più attributi di un utente di accesso deve essere un membro di tutti i ruoli specificato. l'esempio seguente richiede che un utente deve essere un membro di entrambi i `PowerUser` e `ControlPanelUser` ruolo.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-112">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="cc9f9-113">È possibile limitare ulteriormente l'accesso tramite l'applicazione di attributi di autorizzazione di ruolo aggiuntivi a livello di azione;</span><span class="sxs-lookup"><span data-stu-id="cc9f9-113">You can further limit access by applying additional role authorization attributes at the action level;</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="cc9f9-114">Nei membri di frammento di codice precedente di `Administrator` ruolo o `PowerUser` ruolo può accedere al controller e `SetTime` azione, ma solo i membri del `Administrator` ruolo può accedere il `ShutDown` azione.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-114">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="cc9f9-115">È inoltre possibile bloccare un controller di ma consentire l'accesso anonimo, non autenticato a singole azioni.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-115">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name=security-authorization-role-policy></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="cc9f9-116">Controlli del ruolo basata su criteri</span><span class="sxs-lookup"><span data-stu-id="cc9f9-116">Policy based role checks</span></span>

<span data-ttu-id="cc9f9-117">Requisiti del ruolo possono essere anche espressa utilizzando la nuova sintassi di criteri, in cui uno sviluppatore registra una politica di avvio come parte della configurazione del servizio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-117">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="cc9f9-118">Questo errore si verifica in genere `ConfigureServices()` nel *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-118">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="cc9f9-119">I criteri vengono applicati utilizzando il `Policy` proprietà il `AuthorizeAttribute` attributo;</span><span class="sxs-lookup"><span data-stu-id="cc9f9-119">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute;</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="cc9f9-120">Se si desidera specificare più ruoli consentiti in un requisito, è possibile specificarli come parametri per il `RequireRole` ; (metodo)</span><span class="sxs-lookup"><span data-stu-id="cc9f9-120">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method;</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="cc9f9-121">In questo esempio autorizza gli utenti che appartengono al `Administrator`, `PowerUser` o `BackupAdministrator` ruoli.</span><span class="sxs-lookup"><span data-stu-id="cc9f9-121">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
