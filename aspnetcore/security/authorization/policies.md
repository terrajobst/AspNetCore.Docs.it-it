---
title: Autorizzazione personalizzata basata su criteri
author: rick-anderson
description: Questo documento illustra come creare e utilizzare i gestori di criteri di autorizzazione personalizzato in un'applicazione ASP.NET Core.
keywords: ASP.NET Core, autorizzazione, i criteri personalizzati, i criteri di autorizzazione
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a>Autorizzazione personalizzata basata su criteri

<a name="security-authorization-policies-based"></a>

Sotto le quinte, il [autorizzazione ruolo](roles.md) e [le attestazioni di autorizzazione](claims.md) verificare l'utilizzo di un requisito, un gestore per il requisito e un criterio configurato in precedenza. Questi blocchi predefiniti consentono di esprimere le valutazioni di autorizzazione nel codice, consentendo più dettagliato, riutilizzabili e nella struttura di autorizzazione facilmente testabili.

Criteri di autorizzazione sono costituita da uno o più requisiti e registrato all'avvio dell'applicazione come parte della configurazione del servizio di autorizzazione, in `ConfigureServices` nel *Startup.cs* file.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

Qui è possibile visualizzare che un criterio di "Over21" viene creato con un unico requisito, che una durata minima, che viene passata come parametro al requisito.

I criteri vengono applicati utilizzando il `Authorize` attributo specificando il nome dei criteri, ad esempio;

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Requisiti

Un requisito di autorizzazione è una raccolta di parametri dei dati che un criterio è possibile utilizzare per valutare l'entità utente corrente. Nel criterio periodo di memorizzazione minimo, il requisito è è un singolo parametro, la data minima. È necessario implementare un requisito `IAuthorizationRequirement`. Si tratta di un'interfaccia vuota, di marcatore. Un requisito di età minima con parametri potrebbe essere implementato come segue:

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

Non è un requisito a dati o proprietà.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestori di autorizzazione

Un gestore di autorizzazione è responsabile per la valutazione di tutte le proprietà di un requisito. Il gestore di autorizzazione necessario valutarli in base a un oggetto fornito `AuthorizationHandlerContext` per decidere se l'autorizzazione è consentita. Può avere un requisito [più gestori](policies.md#security-authorization-policies-based-multiple-handlers). I gestori devono ereditare `AuthorizationHandler<T>` dove T è il requisito gestisce.

<a name="security-authorization-handler-example"></a>

Il gestore di età minima potrebbe essere simile al seguente:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Nel codice precedente, viene innanzitutto analizzato per vedere se l'entità utente corrente dispone di una data di nascita di attestazioni che è stata eseguita da un'autorità di certificazione è noto e attendibile. Se manca l'attestazione è Impossibile autorizzare in modo verrà restituito. Se si dispone di un'attestazione è capire risale l'utente e se soddisfano la data minima passata dal requisito quindi autorizzazione è stata completata. Una volta che viene concessa autorizzazione definiamo `context.Succeed()` passando il requisito che è stato eseguito correttamente come parametro.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registrazione del gestore
Gestori eventi devono essere registrati nella raccolta di servizi durante la configurazione, ad esempio,

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Ogni gestore viene aggiunto alla raccolta di servizi tramite `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passando la classe del gestore.

## <a name="what-should-a-handler-return"></a>Cosa deve restituire un gestore?

È possibile visualizzare nel nostro [esempio gestore](policies.md#security-authorization-handler-example) che il `Handle()` metodo non restituisce alcun valore, in che modo si indica esito positivo o negativo?

* Indica l'esito positivo chiamando un gestore `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stata convalidata.

* Un gestore non è necessario gestire gli errori in genere, come altri gestori per lo stesso requisito può essere completata.

* Errore anche se altri gestori per un requisito di esito positivo, chiamare `context.Fail`.

Indipendentemente dal fatto ciò che è definito all'interno del gestore, tutti i gestori per un requisito verranno chiamati quando un criterio richiede il requisito. In questo modo i requisiti di effetti collaterali, ad esempio la registrazione, che verrà sempre eseguita anche se `context.Fail()` è stato chiamato in un altro gestore.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Perché desiderare più gestori per un requisito?

Nei casi in cui si desidera valutazione in un **o** è implementare più gestori per un unico requisito di base. Microsoft ha ad esempio, le porte che viene aperta solo con schede di chiave. Se si lascia la smart card a casa il centralinista Stampa etichetta della custodia temporanea e verrà aperta la porta. In questo scenario è necessario un unico requisito *EnterBuilding*, ma più gestori, ciascuno di essi un unico requisito di analisi.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

A questo punto, supponendo che entrambi i gestori sono [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) quando un criterio restituisce il `EnterBuildingRequirement` se ha esito positivo su gestore valutazione dei criteri avrà esito positivo.

## <a name="using-a-func-to-fulfill-a-policy"></a>Utilizzo di func per soddisfare i criteri di

Potrebbe accadere che soddisfano un criterio in semplice per esprimere nel codice. È possibile specificare semplicemente un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.

Ad esempio precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>Accesso al contesto di richiesta MVC nei gestori

Il `Handle` (metodo) deve implementare un gestore di autorizzazione ha due parametri, un `AuthorizationContext` e `Requirement` si sta gestendo. Framework di MVC o Jabbr hanno la possibilità di aggiungere qualsiasi oggetto di `Resource` proprietà di `AuthorizationContext` passare informazioni aggiuntive.

Ad esempio, MVC passa un'istanza di `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` nella proprietà della risorsa che viene utilizzata per accedere HttpContext, RouteData e tutti gli elementi else MVC fornisce.

L'utilizzo del `Resource` è di proprietà specifici del framework. Utilizzando le informazioni nel `Resource` proprietà limiterà i criteri di autorizzazione per un framework specifico. È necessario eseguire il cast di `Resource` proprietà utilizzando il `as` (parola chiave) e quindi controllare il cast ha esito positivo per verificare che il codice non viene arrestato in modo anomalo con `InvalidCastExceptions` quando viene eseguito da altri Framework;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
