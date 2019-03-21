---
title: Autorizzazione basata sui ruoli in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare l'accesso di azione e del controller ASP.NET Core mediante il passaggio di ruoli per l'attributo Authorize.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0e01e1976e2721ca64720a67c6341661f646395c
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209096"
---
# <a name="role-based-authorization-in-aspnet-core"></a>Autorizzazione basata sui ruoli in ASP.NET Core

<a name="security-authorization-role-based"></a>

Quando viene creata un'identità può appartenere a uno o più ruoli. Ad esempio, Tracy possono appartenere ai ruoli di amministratore e utente pur Scott può appartenere solo al ruolo utente. Modo in cui questi ruoli vengono creati e gestiti dipende dall'archivio di backup del processo di autorizzazione. I ruoli vengono esposte agli sviluppatori tramite il [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodo sulle [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) classe.

## <a name="adding-role-checks"></a>Aggiunta di controlli del ruolo

Controlli di autorizzazione basata sui ruoli sono dichiarativi&mdash;lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, che specifica i ruoli che l'utente corrente deve essere un membro di accedere alla risorsa richiesta.

Ad esempio, il codice seguente limita l'accesso a tutte le azioni nel `AdministrationController` agli utenti che sono membri del `Administrator` ruolo:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

È possibile specificare più ruoli come elenco delimitato da virgole:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Questo controller possono essere solo accessibile dagli utenti che sono membri del `HRManager` ruolo o `Finance` ruolo.

Se si applicano più attributi di un utente di accesso deve essere un membro di tutti i ruoli specificati; l'esempio seguente richiede che un utente deve essere un membro di entrambi i `PowerUser` e `ControlPanelUser` ruolo.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

È possibile limitare ulteriormente l'accesso tramite l'applicazione di attributi di autorizzazione di ruolo aggiuntive a livello di azione:

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

Nei membri di frammento di codice precedente del `Administrator` ruolo o il `PowerUser` ruolo può accedere al controller e il `SetTime` azione, ma solo i membri del `Administrator` ruolo può accedere il `ShutDown` azione.

È anche possibile bloccare un controller ma consentire l'accesso anonimo, non autenticato a singole azioni.

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

Per le pagine Razor, la `AuthorizeAttribute` possono essere applicati in uno dei modi:

* Usando un [convenzione](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), o
* Applicando la `AuthorizeAttribute` per il `PageModel` istanza:

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
> Filtrare gli attributi, tra cui `AuthorizeAttribute`, può essere applicato solo a PageModel e non può essere applicato ai metodi del gestore di pagina specifica.
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>Controlli di ruolo basata su criteri

Requisiti del ruolo possono essere espresso anche usando la nuova sintassi di criteri, in cui lo sviluppatore registra un criterio all'avvio come parte della configurazione del servizio di autorizzazione. Ciò si verifica in genere nelle `ConfigureServices()` nella *Startup.cs* file.

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

I criteri vengono applicati utilizzando la `Policy` proprietà di `AuthorizeAttribute` attributo:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Se si desidera specificare più ruoli consentiti in un requisito, è possibile specificarli come parametri per il `RequireRole` metodo:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

In questo esempio autorizza gli utenti che appartengono al `Administrator`, `PowerUser` o `BackupAdministrator` ruoli.

### <a name="add-role-services-to-identity"></a>Aggiungere altri servizi di identità

Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
