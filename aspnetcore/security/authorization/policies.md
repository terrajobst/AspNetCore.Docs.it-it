---
title: Autorizzazione personalizzata basata su criteri
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 24585ed5b4c21a357fc0eed4de6ccedf9fa50d3e
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="e9203-103">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="e9203-103">Custom Policy-Based Authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="e9203-104">Nel sistema l'il [autorizzazione ruolo](roles.md) e [le attestazioni di autorizzazione](claims.md) verificare l'utilizzo di un requisito, un gestore per il requisito e i criteri configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e9203-104">Underneath the covers the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement and a pre-configured policy.</span></span> <span data-ttu-id="e9203-105">Questi blocchi predefiniti consentono di esprimere le valutazioni di autorizzazione nel codice, consentendo più dettagliato, riutilizzabili e nella struttura di autorizzazione facilmente testabili.</span><span class="sxs-lookup"><span data-stu-id="e9203-105">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="e9203-106">Criteri di autorizzazione sono costituita da uno o più requisiti e registrato all'avvio dell'applicazione come parte della configurazione del servizio di autorizzazione, in `ConfigureServices` nel *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="e9203-106">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

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

<span data-ttu-id="e9203-107">Qui è possibile visualizzare che un criterio di "Over21" viene creato con un unico requisito, che una durata minima, che viene passata come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="e9203-107">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="e9203-108">I criteri vengono applicati utilizzando il `Authorize` attributo specificando il nome dei criteri, ad esempio;</span><span class="sxs-lookup"><span data-stu-id="e9203-108">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

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

## <a name="requirements"></a><span data-ttu-id="e9203-109">Requisiti</span><span class="sxs-lookup"><span data-stu-id="e9203-109">Requirements</span></span>

<span data-ttu-id="e9203-110">Un requisito di autorizzazione è una raccolta di parametri dei dati che un criterio è possibile utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="e9203-110">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="e9203-111">Nei criteri di durata minima il requisito è è un singolo parametro, la data minima.</span><span class="sxs-lookup"><span data-stu-id="e9203-111">In our Minimum Age policy the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="e9203-112">È necessario implementare un requisito `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="e9203-112">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="e9203-113">Si tratta di un'interfaccia vuota, di marcatore.</span><span class="sxs-lookup"><span data-stu-id="e9203-113">This is an empty, marker interface.</span></span> <span data-ttu-id="e9203-114">Un requisito di età minima con parametri potrebbe essere implementato come segue:</span><span class="sxs-lookup"><span data-stu-id="e9203-114">A parameterized minimum age requirement might be implemented as follows;</span></span>

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

<span data-ttu-id="e9203-115">Non è un requisito a dati o proprietà.</span><span class="sxs-lookup"><span data-stu-id="e9203-115">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="e9203-116">Gestori di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="e9203-116">Authorization Handlers</span></span>

<span data-ttu-id="e9203-117">Un gestore di autorizzazione è responsabile per la valutazione di tutte le proprietà di un requisito.</span><span class="sxs-lookup"><span data-stu-id="e9203-117">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="e9203-118">Il gestore di autorizzazione necessario valutarli in base a un oggetto fornito `AuthorizationHandlerContext` per decidere se l'autorizzazione è consentita.</span><span class="sxs-lookup"><span data-stu-id="e9203-118">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="e9203-119">Può avere un requisito [più gestori](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="e9203-119">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="e9203-120">I gestori devono ereditare `AuthorizationHandler<T>` dove T è il requisito gestisce.</span><span class="sxs-lookup"><span data-stu-id="e9203-120">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="e9203-121">Il gestore di età minima potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e9203-121">The minimum age handler might look like this:</span></span>

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

<span data-ttu-id="e9203-122">Nel codice sopra viene innanzitutto analizzato per vedere se l'entità utente corrente dispone di una data di nascita di attestazioni che è stata eseguita da un'autorità di certificazione è noto e attendibile.</span><span class="sxs-lookup"><span data-stu-id="e9203-122">In the code above we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="e9203-123">Se manca l'attestazione è Impossibile autorizzare in modo verrà restituito.</span><span class="sxs-lookup"><span data-stu-id="e9203-123">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="e9203-124">Se si dispone di un'attestazione è capire risale l'utente e se soddisfano la data minima passata dal requisito quindi autorizzazione è stata completata.</span><span class="sxs-lookup"><span data-stu-id="e9203-124">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="e9203-125">Una volta che viene concessa autorizzazione definiamo `context.Succeed()` passando il requisito che è stato eseguito correttamente come parametro.</span><span class="sxs-lookup"><span data-stu-id="e9203-125">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

<span data-ttu-id="e9203-126">Gestori eventi devono essere registrati nella raccolta di servizi durante la configurazione, ad esempio,</span><span class="sxs-lookup"><span data-stu-id="e9203-126">Handlers must be registered in the services collection during configuration, for example;</span></span>

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

<span data-ttu-id="e9203-127">Ogni gestore viene aggiunto alla raccolta di servizi tramite `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passando la classe del gestore.</span><span class="sxs-lookup"><span data-stu-id="e9203-127">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="e9203-128">Cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="e9203-128">What should a handler return?</span></span>

<span data-ttu-id="e9203-129">È possibile visualizzare nel nostro [esempio gestore](policies.md#security-authorization-handler-example) che il `Handle()` metodo non restituisce alcun valore, in che modo si indica esito positivo o negativo?</span><span class="sxs-lookup"><span data-stu-id="e9203-129">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="e9203-130">Indica l'esito positivo chiamando un gestore `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stata convalidata.</span><span class="sxs-lookup"><span data-stu-id="e9203-130">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="e9203-131">Un gestore non è necessario gestire gli errori in genere, come altri gestori per lo stesso requisito può essere completata.</span><span class="sxs-lookup"><span data-stu-id="e9203-131">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="e9203-132">Errore anche se altri gestori per un requisito di esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="e9203-132">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="e9203-133">Indipendentemente dal fatto si chiama il gestore all'interno di tutti i gestori per un requisito verranno chiamati quando un criterio richiede il requisito.</span><span class="sxs-lookup"><span data-stu-id="e9203-133">Regardless of what you call inside your handler all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="e9203-134">In questo modo i requisiti di effetti collaterali, ad esempio la registrazione, che verrà sempre eseguita anche se `context.Fail()` è stato chiamato in un altro gestore.</span><span class="sxs-lookup"><span data-stu-id="e9203-134">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="e9203-135">Perché desiderare più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="e9203-135">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="e9203-136">Nei casi in cui si desidera valutazione in un **o** è implementare più gestori per un unico requisito di base.</span><span class="sxs-lookup"><span data-stu-id="e9203-136">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="e9203-137">Microsoft ha ad esempio, le porte che viene aperta solo con schede di chiave.</span><span class="sxs-lookup"><span data-stu-id="e9203-137">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="e9203-138">Se si lascia la smart card a casa il centralinista Stampa etichetta della custodia temporanea e verrà aperta la porta.</span><span class="sxs-lookup"><span data-stu-id="e9203-138">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="e9203-139">In questo scenario è necessario un unico requisito *EnterBuilding*, ma più gestori, ciascuno di essi un unico requisito di analisi.</span><span class="sxs-lookup"><span data-stu-id="e9203-139">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

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

<span data-ttu-id="e9203-140">A questo punto, supponendo che entrambi i gestori sono [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) quando un criterio restituisce il `EnterBuildingRequirement` se ha esito positivo su gestore valutazione dei criteri avrà esito positivo.</span><span class="sxs-lookup"><span data-stu-id="e9203-140">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="e9203-141">Utilizzo di func per soddisfare i criteri di</span><span class="sxs-lookup"><span data-stu-id="e9203-141">Using a func to fulfill a policy</span></span>

<span data-ttu-id="e9203-142">Potrebbe accadere che soddisfano un criterio in semplice per esprimere nel codice.</span><span class="sxs-lookup"><span data-stu-id="e9203-142">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="e9203-143">È possibile specificare semplicemente un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.</span><span class="sxs-lookup"><span data-stu-id="e9203-143">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="e9203-144">Ad esempio precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="e9203-144">For example the previous `BadgeEntryHandler` could be rewritten as follows;</span></span>

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

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="e9203-145">Accesso al contesto di richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="e9203-145">Accessing MVC Request Context In Handlers</span></span>

<span data-ttu-id="e9203-146">Il `Handle` (metodo) deve implementare un gestore di autorizzazione ha due parametri, un `AuthorizationContext` e `Requirement` si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="e9203-146">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="e9203-147">Framework di MVC o Jabbr hanno la possibilità di aggiungere qualsiasi oggetto di `Resource` proprietà di `AuthorizationContext` passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="e9203-147">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="e9203-148">Ad esempio MVC passa un'istanza di `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` nella proprietà della risorsa che viene utilizzata per accedere HttpContext, RouteData e tutti gli elementi else MVC fornisce.</span><span class="sxs-lookup"><span data-stu-id="e9203-148">For example MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="e9203-149">L'utilizzo del `Resource` è di proprietà specifici del framework.</span><span class="sxs-lookup"><span data-stu-id="e9203-149">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="e9203-150">Utilizzando le informazioni nel `Resource` proprietà limiterà i criteri di autorizzazione per un framework specifico.</span><span class="sxs-lookup"><span data-stu-id="e9203-150">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="e9203-151">È necessario eseguire il cast di `Resource` proprietà utilizzando il `as` (parola chiave) e quindi controllare il cast ha esito positivo per verificare che il codice non viene arrestato in modo anomalo con `InvalidCastExceptions` quando viene eseguito da altri Framework;</span><span class="sxs-lookup"><span data-stu-id="e9203-151">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
