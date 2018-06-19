---
title: Autorizzazione basata sulle attestazioni in ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere i controlli di attestazioni per l'autorizzazione in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34336304"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autorizzazione basata sulle attestazioni in ASP.NET Core

<a name="security-authorization-claims-based"></a>

Quando viene creata un'identità è possibile assegnare una o più attestazioni emesso da un'entità attendibile. Un'attestazione è una coppia nome / valore che rappresenta il soggetto è, non quali l'oggetto è possibile farlo. Ad esempio, è possibile di Guida patente, emesso da un'autorità di licenza Guida locale. Di Guida la patente presenta la data di nascita. In questo caso il nome di attestazione sarebbe `DateOfBirth`, il valore dell'attestazione sarebbe ad esempio la data di nascita, `8th June 1970` e l'autorità emittente sarebbe l'autorità di licenza determinante. Nella forma più semplice, l'autorizzazione basata sulle attestazioni, controlla il valore di un'attestazione e consente l'accesso a una risorsa in base al valore. Per esempio, se si desidera che l'accesso a un'associazione di notte il processo di autorizzazione potrebbe essere:

Responsabile della sicurezza della porta restituirà il valore della tua data di nascita attestazione e se devono considerare attendibili l'autorità emittente (l'autorità licenza determinante) prima che concede che l'accesso.

Un'identità può contenere più attestazioni con più valori e può contenere più attestazioni dello stesso tipo.

## <a name="adding-claims-checks"></a>Aggiunta di controlli di attestazioni

Attestazione controlli di autorizzazione di base sono dichiarativi, lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, specificare le attestazioni che l'utente corrente deve possedere e, facoltativamente, il valore attestazione deve contenere per l'accesso di risorsa richiesta. Attestazioni requisiti sono basati su criteri, lo sviluppatore deve creare e registrare un criterio di esprimere i requisiti di attestazioni.

Il tipo più semplice di criteri Cerca la presenza di un'attestazione di attestazione e non verifica il valore.

Innanzitutto è necessario compilare e registrare i criteri. Il processo viene eseguito come parte della configurazione del servizio di autorizzazione, che in genere fa parte di `ConfigureServices()` nel *Startup.cs* file.

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

In questo caso il `EmployeeOnly` criteri consentono di controllare la presenza di un `EmployeeNumber` attestazione all'identità corrente.

È quindi applicare il criterio utilizzando il `Policy` proprietà il `AuthorizeAttribute` attributo per specificare il nome dei criteri;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

Il `AuthorizeAttribute` attributo può essere applicato a un controller intero, in questa istanza, solo i criteri di corrispondenza di identità potrà accedere a qualsiasi azione nel controller.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Se si dispone di un controller che è protetta dal `AuthorizeAttribute` attributo, ma si desidera consentire l'accesso anonimo a specifiche azioni si applica il `AllowAnonymousAttribute` attributo.

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

La maggior parte delle attestazioni vengono forniti con un valore. Quando si crea il criterio, è possibile specificare un elenco di valori consentiti. Nell'esempio seguente viene completata solo per i dipendenti il cui numero di dipendenti è 1, 2, 3, 4 o 5.

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

Se il valore dell'attestazione non è un valore singolo o una trasformazione è necessario, utilizzare [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Per altre informazioni, vedere [utilizzando un oggetto func per soddisfare un criterio](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Più la valutazione dei criteri

Se più criteri si applicano a un controller o un'azione, tutti i criteri devono passare prima che venga concesso l'accesso. Ad esempio:

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

Nell'esempio precedente qualsiasi identità che soddisfa la `EmployeeOnly` criteri possono accedere il `Payslip` azione come tale criterio viene imposto per il controller. Tuttavia per chiamare il `UpdateSalary` azione deve soddisfare l'identità *entrambi* il `EmployeeOnly` criteri e `HumanResources` criteri.

Se si desidera criteri più complessi, ad esempio l'esecuzione di una data di nascita attestazione, il calcolo di un'età da quest'ultimo verifica la validità è 21 o meno recenti quindi è necessario scrivere [gestori di criteri personalizzati](xref:security/authorization/policies).
