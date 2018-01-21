---
title: Autorizzazione basata sui ruoli
author: rick-anderson
description: Questo documento viene illustrato come limitare l'accesso di azione e del controller di ASP.NET Core passando i ruoli per l'attributo Authorize.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 18964464fea76c91d716202d89ee3a3eb36c3078
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="role-based-authorization"></a>Autorizzazione basata sui ruoli

<a name="security-authorization-role-based"></a>

Quando viene creata un'identità può appartenere a uno o più ruoli. Ad esempio, Tracy può appartenere ai ruoli di amministratore e utente pur Scott può appartenere solo al ruolo utente. Come questi ruoli vengono creati e gestiti dipende dall'archivio di backup del processo di autorizzazione. I ruoli vengono esposte allo sviluppatore di tramite il [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) metodo il [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) classe.

## <a name="adding-role-checks"></a>Aggiunta di controlli del ruolo

Controlli di autorizzazione basata sui ruoli sono dichiarativi&mdash;lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, specificare i ruoli che l'utente corrente deve essere un membro di accedere alla risorsa richiesta.

Ad esempio, il codice seguente limita l'accesso a qualsiasi azione sul `AdministrationController` agli utenti che sono membri del `Administrator` ruolo:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

È possibile specificare più ruoli come un elenco separato da virgole:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Questo controller possono essere solo accessibile dagli utenti che sono membri del `HRManager` ruolo o `Finance` ruolo.

Se si applicano più attributi di un utente di accesso deve essere un membro di tutti i ruoli specificato. l'esempio seguente richiede che un utente deve essere un membro di entrambi i `PowerUser` e `ControlPanelUser` ruolo.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

È possibile limitare ulteriormente l'accesso tramite l'applicazione di attributi di autorizzazione di ruolo aggiuntivi a livello di azione:

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

Nei membri di frammento di codice precedente di `Administrator` ruolo o `PowerUser` ruolo può accedere al controller e `SetTime` azione, ma solo i membri del `Administrator` ruolo può accedere il `ShutDown` azione.

È inoltre possibile bloccare un controller di ma consentire l'accesso anonimo, non autenticato a singole azioni.

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

## <a name="policy-based-role-checks"></a>Controlli del ruolo basata su criteri

Requisiti del ruolo possono essere anche espressa utilizzando la nuova sintassi di criteri, in cui uno sviluppatore registra una politica di avvio come parte della configurazione del servizio di autorizzazione. Questo errore si verifica in genere `ConfigureServices()` nel *Startup.cs* file.

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

I criteri vengono applicati utilizzando il `Policy` proprietà il `AuthorizeAttribute` attributo:

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
