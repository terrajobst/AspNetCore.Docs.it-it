---
title: Autorizzazione basata sui ruoli in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare l'accesso di azione e del controller ASP.NET Core mediante il passaggio di ruoli per l'attributo Authorize.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: c38e7144166ce7741eee6e3acb4d1c952ad4f024
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899164"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="b6472-103">Autorizzazione basata sui ruoli in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6472-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="b6472-104">Quando viene creata un'identità può appartenere a uno o più ruoli.</span><span class="sxs-lookup"><span data-stu-id="b6472-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="b6472-105">Ad esempio, Tracy possono appartenere ai ruoli di amministratore e utente pur Scott può appartenere solo al ruolo utente.</span><span class="sxs-lookup"><span data-stu-id="b6472-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="b6472-106">Modo in cui questi ruoli vengono creati e gestiti dipende dall'archivio di backup del processo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="b6472-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="b6472-107">I ruoli vengono esposte agli sviluppatori tramite il [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodo sulle [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) classe.</span><span class="sxs-lookup"><span data-stu-id="b6472-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="b6472-108">Aggiunta di controlli del ruolo</span><span class="sxs-lookup"><span data-stu-id="b6472-108">Adding role checks</span></span>

<span data-ttu-id="b6472-109">Controlli di autorizzazione basata sui ruoli sono dichiarativi&mdash;lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, che specifica i ruoli che l'utente corrente deve essere un membro di accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="b6472-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="b6472-110">Ad esempio, il codice seguente limita l'accesso a tutte le azioni nel `AdministrationController` agli utenti che sono membri del `Administrator` ruolo:</span><span class="sxs-lookup"><span data-stu-id="b6472-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="b6472-111">È possibile specificare più ruoli come elenco delimitato da virgole:</span><span class="sxs-lookup"><span data-stu-id="b6472-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="b6472-112">Questo controller possono essere solo accessibile dagli utenti che sono membri del `HRManager` ruolo o `Finance` ruolo.</span><span class="sxs-lookup"><span data-stu-id="b6472-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="b6472-113">Se si applicano più attributi di un utente di accesso deve essere un membro di tutti i ruoli specificati; l'esempio seguente richiede che un utente deve essere un membro di entrambi i `PowerUser` e `ControlPanelUser` ruolo.</span><span class="sxs-lookup"><span data-stu-id="b6472-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="b6472-114">È possibile limitare ulteriormente l'accesso tramite l'applicazione di attributi di autorizzazione di ruolo aggiuntive a livello di azione:</span><span class="sxs-lookup"><span data-stu-id="b6472-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="b6472-115">Nei membri di frammento di codice precedente del `Administrator` ruolo o il `PowerUser` ruolo può accedere al controller e il `SetTime` azione, ma solo i membri del `Administrator` ruolo può accedere il `ShutDown` azione.</span><span class="sxs-lookup"><span data-stu-id="b6472-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="b6472-116">È anche possibile bloccare un controller ma consentire l'accesso anonimo, non autenticato a singole azioni.</span><span class="sxs-lookup"><span data-stu-id="b6472-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b6472-117">Per le pagine Razor, la `AuthorizeAttribute` possono essere applicati in uno dei modi:</span><span class="sxs-lookup"><span data-stu-id="b6472-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="b6472-118">Usando un [convenzione](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), o</span><span class="sxs-lookup"><span data-stu-id="b6472-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="b6472-119">Applicando la `AuthorizeAttribute` per il `PageModel` istanza:</span><span class="sxs-lookup"><span data-stu-id="b6472-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public class UpdateModel : PageModel
{
    public ActionResult OnPost()
    {
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="b6472-120">Filtrare gli attributi, tra cui `AuthorizeAttribute`, può essere applicato solo a PageModel e non può essere applicato ai metodi del gestore di pagina specifica.</span><span class="sxs-lookup"><span data-stu-id="b6472-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end


<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="b6472-121">Controlli di ruolo basata su criteri</span><span class="sxs-lookup"><span data-stu-id="b6472-121">Policy based role checks</span></span>

<span data-ttu-id="b6472-122">Requisiti del ruolo possono essere espresso anche usando la nuova sintassi di criteri, in cui lo sviluppatore registra un criterio all'avvio come parte della configurazione del servizio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="b6472-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="b6472-123">Ciò si verifica in genere nelle `ConfigureServices()` nella *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="b6472-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="b6472-124">I criteri vengono applicati utilizzando la `Policy` proprietà di `AuthorizeAttribute` attributo:</span><span class="sxs-lookup"><span data-stu-id="b6472-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="b6472-125">Se si desidera specificare più ruoli consentiti in un requisito, è possibile specificarli come parametri per il `RequireRole` metodo:</span><span class="sxs-lookup"><span data-stu-id="b6472-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="b6472-126">In questo esempio autorizza gli utenti che appartengono al `Administrator`, `PowerUser` o `BackupAdministrator` ruoli.</span><span class="sxs-lookup"><span data-stu-id="b6472-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="b6472-127">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="b6472-127">Add Role services to Identity</span></span>

<span data-ttu-id="b6472-128">Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="b6472-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
