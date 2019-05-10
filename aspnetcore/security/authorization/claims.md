---
title: Autorizzazione basata sulle attestazioni in ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere controlli di attestazioni per l'autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086159"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autorizzazione basata sulle attestazioni in ASP.NET Core

<a name="security-authorization-claims-based"></a>

Quando viene creata un'identità è possibile assegnare uno o più delle attestazioni rilasciate da un'entità attendibile. Un'attestazione è una coppia nome-valore che rappresenta il soggetto è, non quali il soggetto è possibile farlo. Ad esempio, potrebbe essere di Guida una patente, emesso da un'autorità di licenza Guida locale. Di Guida la patente è la data di nascita su di esso. In questo caso sarebbe il nome dell'attestazione `DateOfBirth`, il valore dell'attestazione sarà la data di nascita, ad esempio `8th June 1970` e l'autorità di certificazione sarebbe l'autorità di licenza determinante. Autorizzazione basata sulle attestazioni, nella forma più semplice, controlla il valore dell'attestazione e consente l'accesso a una risorsa in base al valore. Per esempio, se si desidera che l'accesso a un club di notte il processo di autorizzazione potrebbe essere:

Il responsabile della sicurezza sportello restituirà il valore della data di nascita attestazione e se sono attendibili l'autorità emittente (l'autorità licenza determinante) prima che concede che l'accesso.

Un'identità può contenere più attestazioni con più valori e può contenere più attestazioni dello stesso tipo.

## <a name="adding-claims-checks"></a>Aggiunta di controlli di attestazioni

Attestazione controlli di autorizzazione basata su sono dichiarativi, lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, che specifica le attestazioni che l'utente corrente deve disporre del privilegio e, facoltativamente, il valore di attestazione deve contenere per l'accesso di risorsa richiesta. I requisiti sono criteri basati su attestazioni, lo sviluppatore deve creare e registrare un criterio che esprimono i requisiti di attestazioni.

Il tipo più semplice di criteri Cerca la presenza di un'attestazione di attestazione e non controlla il valore.

Prima di tutto è necessario compilare e registrare i criteri. Questa operazione viene eseguita come parte della configurazione del servizio di autorizzazione, che in genere fa parte di `ConfigureServices()` nella *Startup.cs* file.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

In questo caso il `EmployeeOnly` criteri consentono di controllare la presenza di un `EmployeeNumber` attestazioni sull'identità corrente.

È quindi applicare il criterio tramite il `Policy` proprietà il `AuthorizeAttribute` attributo per specificare il nome del criterio;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

Il `AuthorizeAttribute` attributo può essere applicato a un intero controller, in questo caso solo le identità di criteri di corrispondenza potrà accedere a qualsiasi azione sul controller.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Se si dispone di un controller protetto con il `AuthorizeAttribute` dell'attributo, ma si vuole consentire l'accesso anonimo ad azioni specifiche si applica il `AllowAnonymousAttribute` attributo.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

La maggior parte delle attestazioni dotati di un valore. Quando si crea il criterio, è possibile specificare un elenco di valori consentiti. Nell'esempio seguente viene completata solo per i dipendenti il cui numero di dipendenti è 1, 2, 3, 4 o 5.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

### <a name="add-a-generic-claim-check"></a>Aggiungere un controllo di attestazione generico

Se il valore dell'attestazione non è un singolo valore o una trasformazione è obbligatorio, usare [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Per altre informazioni, vedere [usando una funzione per soddisfare un criterio](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Valutazione dei criteri più

Se si applicano più criteri in un controller o azione, tutti i criteri devono passare prima che venga concesso l'accesso. Ad esempio:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

Nell'esempio precedente qualsiasi entità che soddisfa la `EmployeeOnly` criteri possono accedere il `Payslip` azione come tale criterio viene applicato nel controller. Tuttavia affinché la chiamata di `UpdateSalary` azione l'identità deve soddisfare *entrambi* il `EmployeeOnly` criteri e il `HumanResources` criteri.

Se si desidera che i criteri più complicati, ad esempio richiedendo una data di nascita attestazione, calcolando un'età da quest'ultimo, quindi verifica la validità è 21 o versione precedente, è necessario scrivere [gestori di criteri personalizzate](xref:security/authorization/policies).
