---
title: Autorizzazione basata sui ruoli in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare l'accesso di azione e del controller di ASP.NET Core passando i ruoli per l'attributo Authorize.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: f1e7209cae1e2a58ad536547d655dd744ca0d3f7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740036"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="befd8-103">Autorizzazione basata sui ruoli in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="befd8-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="befd8-104">Quando viene creata un'identità può appartenere a uno o più ruoli.</span><span class="sxs-lookup"><span data-stu-id="befd8-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="befd8-105">Ad esempio, Tracy può appartenere ai ruoli di amministratore e utente pur Scott può appartenere solo al ruolo utente.</span><span class="sxs-lookup"><span data-stu-id="befd8-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="befd8-106">Come questi ruoli vengono creati e gestiti dipende dall'archivio di backup del processo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="befd8-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="befd8-107">I ruoli vengono esposte allo sviluppatore di tramite il [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodo il [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) classe.</span><span class="sxs-lookup"><span data-stu-id="befd8-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="befd8-108">Aggiunta di controlli del ruolo</span><span class="sxs-lookup"><span data-stu-id="befd8-108">Adding role checks</span></span>

<span data-ttu-id="befd8-109">Controlli di autorizzazione basata sui ruoli sono dichiarativi&mdash;lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, specificare i ruoli che l'utente corrente deve essere un membro di accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="befd8-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="befd8-110">Ad esempio, il codice seguente limita l'accesso a qualsiasi azione sul `AdministrationController` agli utenti che sono membri del `Administrator` ruolo:</span><span class="sxs-lookup"><span data-stu-id="befd8-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="befd8-111">È possibile specificare più ruoli come un elenco separato da virgole:</span><span class="sxs-lookup"><span data-stu-id="befd8-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="befd8-112">Questo controller possono essere solo accessibile dagli utenti che sono membri del `HRManager` ruolo o `Finance` ruolo.</span><span class="sxs-lookup"><span data-stu-id="befd8-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="befd8-113">Se si applicano più attributi di un utente di accesso deve essere un membro di tutti i ruoli specificato. l'esempio seguente richiede che un utente deve essere un membro di entrambi i `PowerUser` e `ControlPanelUser` ruolo.</span><span class="sxs-lookup"><span data-stu-id="befd8-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="befd8-114">È possibile limitare ulteriormente l'accesso tramite l'applicazione di attributi di autorizzazione di ruolo aggiuntivi a livello di azione:</span><span class="sxs-lookup"><span data-stu-id="befd8-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="befd8-115">Nei membri di frammento di codice precedente di `Administrator` ruolo o `PowerUser` ruolo può accedere al controller e `SetTime` azione, ma solo i membri del `Administrator` ruolo può accedere il `ShutDown` azione.</span><span class="sxs-lookup"><span data-stu-id="befd8-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="befd8-116">È inoltre possibile bloccare un controller di ma consentire l'accesso anonimo, non autenticato a singole azioni.</span><span class="sxs-lookup"><span data-stu-id="befd8-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="befd8-117">Controlli del ruolo basata su criteri</span><span class="sxs-lookup"><span data-stu-id="befd8-117">Policy based role checks</span></span>

<span data-ttu-id="befd8-118">Requisiti del ruolo possono essere anche espressa utilizzando la nuova sintassi di criteri, in cui uno sviluppatore registra una politica di avvio come parte della configurazione del servizio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="befd8-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="befd8-119">Questo errore si verifica in genere `ConfigureServices()` nel *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="befd8-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="befd8-120">I criteri vengono applicati utilizzando il `Policy` proprietà il `AuthorizeAttribute` attributo:</span><span class="sxs-lookup"><span data-stu-id="befd8-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="befd8-121">Se si desidera specificare più ruoli consentiti in un requisito, è possibile specificarli come parametri per il `RequireRole` metodo:</span><span class="sxs-lookup"><span data-stu-id="befd8-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="befd8-122">In questo esempio autorizza gli utenti che appartengono al `Administrator`, `PowerUser` o `BackupAdministrator` ruoli.</span><span class="sxs-lookup"><span data-stu-id="befd8-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
