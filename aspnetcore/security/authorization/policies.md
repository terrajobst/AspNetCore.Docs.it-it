---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e usare i gestori dei criteri di autorizzazione per applicare i requisiti di autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828958"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="3331c-103">Autorizzazione basata su criteri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3331c-103">Policy-based authorization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3331c-104">Dietro le quinte, l'autorizzazione [basata sui ruoli](xref:security/authorization/roles) e l' [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore requisiti e un criterio preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="3331c-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="3331c-105">Questi blocchi predefiniti supportano l'espressione delle valutazioni di autorizzazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="3331c-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="3331c-106">Il risultato è una struttura di autorizzazione verificabile, riutilizzabile e più ricca.</span><span class="sxs-lookup"><span data-stu-id="3331c-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="3331c-107">Un criterio di autorizzazione è costituito da uno o più requisiti.</span><span class="sxs-lookup"><span data-stu-id="3331c-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="3331c-108">Viene registrato come parte della configurazione del servizio di autorizzazione, nel metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3331c-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

<span data-ttu-id="3331c-109">Nell'esempio precedente viene creato un criterio "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="3331c-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="3331c-110">Dispone di un singolo requisito&mdash;quello di una validità minima, che viene fornito come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="3331c-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="3331c-111">IAuthorizationService</span></span> 

<span data-ttu-id="3331c-112">Il servizio primario che determina se l'autorizzazione ha esito positivo è <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="3331c-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="3331c-113">Il codice precedente evidenzia i due metodi di [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="3331c-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="3331c-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> è un servizio marcatore senza metodi e il meccanismo per verificare se l'autorizzazione ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="3331c-115">Ogni <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> è responsabile per verificare se sono soddisfatti i requisiti:</span><span class="sxs-lookup"><span data-stu-id="3331c-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="3331c-116">La classe <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> è l'elemento usato dal gestore per contrassegnare se sono stati soddisfatti i requisiti:</span><span class="sxs-lookup"><span data-stu-id="3331c-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="3331c-117">Nel codice seguente viene illustrata l'implementazione predefinita del servizio di autorizzazione semplificata (e annotata con commenti):</span><span class="sxs-lookup"><span data-stu-id="3331c-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="3331c-118">Il codice seguente illustra un tipico `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3331c-118">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

<span data-ttu-id="3331c-119">Utilizzare <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something")]` per l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="3331c-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="3331c-120">Applicazione di criteri ai controller MVC</span><span class="sxs-lookup"><span data-stu-id="3331c-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="3331c-121">Se si usa Razor Pages, vedere [applicazione di criteri ai Razor Pages](#applying-policies-to-razor-pages) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="3331c-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="3331c-122">I criteri vengono applicati ai controller usando l'attributo `[Authorize]` con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="3331c-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="3331c-123">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3331c-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="3331c-124">Applicazione di criteri a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3331c-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="3331c-125">I criteri vengono applicati a Razor Pages usando l'attributo `[Authorize]` con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="3331c-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="3331c-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3331c-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="3331c-127">I criteri possono essere applicati anche a Razor Pages usando una [convenzione di autorizzazione](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="3331c-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="3331c-128">Requisiti di</span><span class="sxs-lookup"><span data-stu-id="3331c-128">Requirements</span></span>

<span data-ttu-id="3331c-129">Un requisito di autorizzazione è una raccolta di parametri dati che un criterio può utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="3331c-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="3331c-130">Nel criterio "AtLeast21", il requisito è un singolo parametro&mdash;età minima.</span><span class="sxs-lookup"><span data-stu-id="3331c-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="3331c-131">Un requisito implementa [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuota.</span><span class="sxs-lookup"><span data-stu-id="3331c-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="3331c-132">Un requisito di validità minima con parametri può essere implementato come segue:</span><span class="sxs-lookup"><span data-stu-id="3331c-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="3331c-133">Se un criterio di autorizzazione contiene più requisiti di autorizzazione, tutti i requisiti devono essere superati affinché la valutazione dei criteri abbia esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="3331c-134">In altre parole, i requisiti di autorizzazione multipli aggiunti a un singolo criterio di autorizzazione vengono gestiti in base a **e** .</span><span class="sxs-lookup"><span data-stu-id="3331c-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="3331c-135">Un requisito non deve avere dati o proprietà.</span><span class="sxs-lookup"><span data-stu-id="3331c-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="3331c-136">Gestori autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="3331c-136">Authorization handlers</span></span>

<span data-ttu-id="3331c-137">Un gestore di autorizzazione è responsabile della valutazione delle proprietà di un requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="3331c-138">Il gestore di autorizzazione valuta i requisiti rispetto a un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) specificato per determinare se l'accesso è consentito.</span><span class="sxs-lookup"><span data-stu-id="3331c-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="3331c-139">Un requisito può avere [più gestori](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="3331c-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="3331c-140">Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito da gestire.</span><span class="sxs-lookup"><span data-stu-id="3331c-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="3331c-141">In alternativa, un gestore può implementare [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="3331c-142">Usare un gestore per un requisito</span><span class="sxs-lookup"><span data-stu-id="3331c-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="3331c-143">Di seguito è riportato un esempio di una relazione uno-a-uno in cui un gestore età minima utilizza un singolo requisito:</span><span class="sxs-lookup"><span data-stu-id="3331c-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="3331c-144">Il codice precedente determina se l'entità utente corrente ha una data di attestazione di nascita che è stata rilasciata da un'autorità emittente nota ed attendibile.</span><span class="sxs-lookup"><span data-stu-id="3331c-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="3331c-145">L'autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata.</span><span class="sxs-lookup"><span data-stu-id="3331c-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="3331c-146">Quando è presente un'attestazione, viene calcolata l'età dell'utente.</span><span class="sxs-lookup"><span data-stu-id="3331c-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="3331c-147">Se l'utente soddisfa la validità minima definita dal requisito, l'autorizzazione viene ritenuta corretta.</span><span class="sxs-lookup"><span data-stu-id="3331c-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="3331c-148">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.</span><span class="sxs-lookup"><span data-stu-id="3331c-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="3331c-149">Usare un gestore per più requisiti</span><span class="sxs-lookup"><span data-stu-id="3331c-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="3331c-150">Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazioni è in grado di gestire tre diversi tipi di requisiti:</span><span class="sxs-lookup"><span data-stu-id="3331c-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="3331c-151">Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene requisiti non contrassegnati come riusciti.</span><span class="sxs-lookup"><span data-stu-id="3331c-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="3331c-152">Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="3331c-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="3331c-153">Nel caso di un requisito `EditPermission` o `DeletePermission`, deve essere un proprietario per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="3331c-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="3331c-154">Registrazione del gestore</span><span class="sxs-lookup"><span data-stu-id="3331c-154">Handler registration</span></span>

<span data-ttu-id="3331c-155">I gestori vengono registrati nella raccolta di servizi durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="3331c-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="3331c-156">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3331c-156">For example:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

<span data-ttu-id="3331c-157">Il codice precedente registra `MinimumAgeHandler` come singleton richiamando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="3331c-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="3331c-158">I gestori possono essere registrati usando una qualsiasi delle [durate dei servizi](xref:fundamentals/dependency-injection#service-lifetimes)predefinite.</span><span class="sxs-lookup"><span data-stu-id="3331c-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="3331c-159">Che cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="3331c-159">What should a handler return?</span></span>

<span data-ttu-id="3331c-160">Si noti che il metodo `Handle` nell' [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="3331c-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="3331c-161">Come viene indicato lo stato di esito positivo o negativo?</span><span class="sxs-lookup"><span data-stu-id="3331c-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="3331c-162">Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stato convalidato correttamente.</span><span class="sxs-lookup"><span data-stu-id="3331c-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="3331c-163">Un gestore non deve gestire gli errori in modo generale, perché gli altri gestori per lo stesso requisito possono avere esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="3331c-164">Per garantire un errore, anche se gli altri gestori di requisiti hanno esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="3331c-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="3331c-165">Se un gestore chiama `context.Succeed` o `context.Fail`, tutti gli altri gestori vengono ancora chiamati.</span><span class="sxs-lookup"><span data-stu-id="3331c-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="3331c-166">Questo consente ai requisiti di produrre effetti collaterali, ad esempio la registrazione, che si verifica anche se un altro gestore ha correttamente convalidato o non ha superato un requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="3331c-167">Se impostato su `false`, la proprietà [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (disponibile in ASP.NET Core 1,1 e versioni successive) esegue il cortocircuito dell'esecuzione dei gestori quando viene chiamato `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="3331c-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="3331c-168">`InvokeHandlersAfterFailure` viene impostato il valore predefinito `true`, nel qual caso vengono chiamati tutti i gestori.</span><span class="sxs-lookup"><span data-stu-id="3331c-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="3331c-169">I gestori di autorizzazione vengono chiamati anche se l'autenticazione ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="3331c-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="3331c-170">Perché si vogliono usare più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="3331c-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="3331c-171">Nei casi in cui si desidera che la valutazione sia basata su **o** , implementare più gestori per un singolo requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="3331c-172">Ad esempio, Microsoft dispone di porte che si aprono solo con le schede chiave.</span><span class="sxs-lookup"><span data-stu-id="3331c-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="3331c-173">Se si lascia la scheda chiave a casa, il receptionist stampa un adesivo temporaneo e apre lo sportello.</span><span class="sxs-lookup"><span data-stu-id="3331c-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="3331c-174">In questo scenario si avrà un singolo requisito, *BuildingEntry*, ma più gestori, ognuno di essi esaminando un singolo requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="3331c-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="3331c-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="3331c-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="3331c-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="3331c-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="3331c-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="3331c-178">Assicurarsi che entrambi i gestori siano [registrati](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="3331c-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="3331c-179">Se uno dei due gestori ha esito positivo quando un criterio valuta il `BuildingEntryRequirement`, la valutazione dei criteri ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="3331c-180">Uso di un Func per soddisfare un criterio</span><span class="sxs-lookup"><span data-stu-id="3331c-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="3331c-181">In alcuni casi è possibile che l'evasione di un criterio sia semplice da esprimere nel codice.</span><span class="sxs-lookup"><span data-stu-id="3331c-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="3331c-182">È possibile specificare un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il generatore di criteri di `RequireAssertion`.</span><span class="sxs-lookup"><span data-stu-id="3331c-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="3331c-183">Ad esempio, il `BadgeEntryHandler` precedente potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="3331c-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="3331c-184">Accesso al contesto della richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="3331c-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="3331c-185">Il metodo `HandleRequirementAsync` implementato in un gestore autorizzazioni presenta due parametri: un `AuthorizationHandlerContext` e il `TRequirement` che si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="3331c-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="3331c-186">I Framework come MVC o Jabbr sono liberi di aggiungere qualsiasi oggetto alla proprietà `Resource` nel `AuthorizationHandlerContext` per passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="3331c-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="3331c-187">Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nella proprietà `Resource`.</span><span class="sxs-lookup"><span data-stu-id="3331c-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="3331c-188">Questa proprietà consente di accedere a `HttpContext`, `RouteData`e tutto il resto fornito da MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3331c-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="3331c-189">L'uso della proprietà `Resource` è specifico del Framework.</span><span class="sxs-lookup"><span data-stu-id="3331c-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="3331c-190">L'uso delle informazioni nella proprietà `Resource` limita i criteri di autorizzazione a specifici Framework.</span><span class="sxs-lookup"><span data-stu-id="3331c-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="3331c-191">È necessario eseguire il cast della proprietà `Resource` usando la parola chiave `is`, quindi verificare che il cast abbia avuto esito positivo per assicurarsi che il codice non si arresti in modo anomalo con un `InvalidCastException` quando viene eseguito in altri Framework:</span><span class="sxs-lookup"><span data-stu-id="3331c-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3331c-192">Dietro le quinte, l'autorizzazione [basata sui ruoli](xref:security/authorization/roles) e l' [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore requisiti e un criterio preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="3331c-192">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="3331c-193">Questi blocchi predefiniti supportano l'espressione delle valutazioni di autorizzazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="3331c-193">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="3331c-194">Il risultato è una struttura di autorizzazione verificabile, riutilizzabile e più ricca.</span><span class="sxs-lookup"><span data-stu-id="3331c-194">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="3331c-195">Un criterio di autorizzazione è costituito da uno o più requisiti.</span><span class="sxs-lookup"><span data-stu-id="3331c-195">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="3331c-196">Viene registrato come parte della configurazione del servizio di autorizzazione, nel metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3331c-196">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="3331c-197">Nell'esempio precedente viene creato un criterio "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="3331c-197">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="3331c-198">Dispone di un singolo requisito&mdash;quello di una validità minima, che viene fornito come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-198">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="3331c-199">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="3331c-199">IAuthorizationService</span></span> 

<span data-ttu-id="3331c-200">Il servizio primario che determina se l'autorizzazione ha esito positivo è <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="3331c-200">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="3331c-201">Il codice precedente evidenzia i due metodi di [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="3331c-201">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="3331c-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> è un servizio marcatore senza metodi e il meccanismo per verificare se l'autorizzazione ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="3331c-203">Ogni <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> è responsabile per verificare se sono soddisfatti i requisiti:</span><span class="sxs-lookup"><span data-stu-id="3331c-203">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="3331c-204">La classe <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> è l'elemento usato dal gestore per contrassegnare se sono stati soddisfatti i requisiti:</span><span class="sxs-lookup"><span data-stu-id="3331c-204">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="3331c-205">Nel codice seguente viene illustrata l'implementazione predefinita del servizio di autorizzazione semplificata (e annotata con commenti):</span><span class="sxs-lookup"><span data-stu-id="3331c-205">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="3331c-206">Il codice seguente illustra un tipico `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3331c-206">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

<span data-ttu-id="3331c-207">Utilizzare <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something")]` per l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="3331c-207">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="3331c-208">Applicazione di criteri ai controller MVC</span><span class="sxs-lookup"><span data-stu-id="3331c-208">Applying policies to MVC controllers</span></span>

<span data-ttu-id="3331c-209">Se si usa Razor Pages, vedere [applicazione di criteri ai Razor Pages](#applying-policies-to-razor-pages) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="3331c-209">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="3331c-210">I criteri vengono applicati ai controller usando l'attributo `[Authorize]` con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="3331c-210">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="3331c-211">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3331c-211">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="3331c-212">Applicazione di criteri a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3331c-212">Applying policies to Razor Pages</span></span>

<span data-ttu-id="3331c-213">I criteri vengono applicati a Razor Pages usando l'attributo `[Authorize]` con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="3331c-213">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="3331c-214">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3331c-214">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="3331c-215">I criteri possono essere applicati anche a Razor Pages usando una [convenzione di autorizzazione](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="3331c-215">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="3331c-216">Requisiti di</span><span class="sxs-lookup"><span data-stu-id="3331c-216">Requirements</span></span>

<span data-ttu-id="3331c-217">Un requisito di autorizzazione è una raccolta di parametri dati che un criterio può utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="3331c-217">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="3331c-218">Nel criterio "AtLeast21", il requisito è un singolo parametro&mdash;età minima.</span><span class="sxs-lookup"><span data-stu-id="3331c-218">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="3331c-219">Un requisito implementa [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuota.</span><span class="sxs-lookup"><span data-stu-id="3331c-219">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="3331c-220">Un requisito di validità minima con parametri può essere implementato come segue:</span><span class="sxs-lookup"><span data-stu-id="3331c-220">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="3331c-221">Se un criterio di autorizzazione contiene più requisiti di autorizzazione, tutti i requisiti devono essere superati affinché la valutazione dei criteri abbia esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-221">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="3331c-222">In altre parole, i requisiti di autorizzazione multipli aggiunti a un singolo criterio di autorizzazione vengono gestiti in base a **e** .</span><span class="sxs-lookup"><span data-stu-id="3331c-222">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="3331c-223">Un requisito non deve avere dati o proprietà.</span><span class="sxs-lookup"><span data-stu-id="3331c-223">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="3331c-224">Gestori autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="3331c-224">Authorization handlers</span></span>

<span data-ttu-id="3331c-225">Un gestore di autorizzazione è responsabile della valutazione delle proprietà di un requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-225">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="3331c-226">Il gestore di autorizzazione valuta i requisiti rispetto a un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) specificato per determinare se l'accesso è consentito.</span><span class="sxs-lookup"><span data-stu-id="3331c-226">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="3331c-227">Un requisito può avere [più gestori](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="3331c-227">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="3331c-228">Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito da gestire.</span><span class="sxs-lookup"><span data-stu-id="3331c-228">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="3331c-229">In alternativa, un gestore può implementare [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-229">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="3331c-230">Usare un gestore per un requisito</span><span class="sxs-lookup"><span data-stu-id="3331c-230">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="3331c-231">Di seguito è riportato un esempio di una relazione uno-a-uno in cui un gestore età minima utilizza un singolo requisito:</span><span class="sxs-lookup"><span data-stu-id="3331c-231">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="3331c-232">Il codice precedente determina se l'entità utente corrente ha una data di attestazione di nascita che è stata rilasciata da un'autorità emittente nota ed attendibile.</span><span class="sxs-lookup"><span data-stu-id="3331c-232">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="3331c-233">L'autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata.</span><span class="sxs-lookup"><span data-stu-id="3331c-233">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="3331c-234">Quando è presente un'attestazione, viene calcolata l'età dell'utente.</span><span class="sxs-lookup"><span data-stu-id="3331c-234">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="3331c-235">Se l'utente soddisfa la validità minima definita dal requisito, l'autorizzazione viene ritenuta corretta.</span><span class="sxs-lookup"><span data-stu-id="3331c-235">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="3331c-236">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.</span><span class="sxs-lookup"><span data-stu-id="3331c-236">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="3331c-237">Usare un gestore per più requisiti</span><span class="sxs-lookup"><span data-stu-id="3331c-237">Use a handler for multiple requirements</span></span>

<span data-ttu-id="3331c-238">Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazioni è in grado di gestire tre diversi tipi di requisiti:</span><span class="sxs-lookup"><span data-stu-id="3331c-238">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="3331c-239">Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene requisiti non contrassegnati come riusciti.</span><span class="sxs-lookup"><span data-stu-id="3331c-239">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="3331c-240">Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="3331c-240">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="3331c-241">Nel caso di un requisito `EditPermission` o `DeletePermission`, deve essere un proprietario per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="3331c-241">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="3331c-242">Registrazione del gestore</span><span class="sxs-lookup"><span data-stu-id="3331c-242">Handler registration</span></span>

<span data-ttu-id="3331c-243">I gestori vengono registrati nella raccolta di servizi durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="3331c-243">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="3331c-244">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3331c-244">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="3331c-245">Il codice precedente registra `MinimumAgeHandler` come singleton richiamando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="3331c-245">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="3331c-246">I gestori possono essere registrati usando una qualsiasi delle [durate dei servizi](xref:fundamentals/dependency-injection#service-lifetimes)predefinite.</span><span class="sxs-lookup"><span data-stu-id="3331c-246">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="3331c-247">Che cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="3331c-247">What should a handler return?</span></span>

<span data-ttu-id="3331c-248">Si noti che il metodo `Handle` nell' [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="3331c-248">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="3331c-249">Come viene indicato lo stato di esito positivo o negativo?</span><span class="sxs-lookup"><span data-stu-id="3331c-249">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="3331c-250">Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stato convalidato correttamente.</span><span class="sxs-lookup"><span data-stu-id="3331c-250">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="3331c-251">Un gestore non deve gestire gli errori in modo generale, perché gli altri gestori per lo stesso requisito possono avere esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-251">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="3331c-252">Per garantire un errore, anche se gli altri gestori di requisiti hanno esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="3331c-252">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="3331c-253">Se un gestore chiama `context.Succeed` o `context.Fail`, tutti gli altri gestori vengono ancora chiamati.</span><span class="sxs-lookup"><span data-stu-id="3331c-253">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="3331c-254">Questo consente ai requisiti di produrre effetti collaterali, ad esempio la registrazione, che si verifica anche se un altro gestore ha correttamente convalidato o non ha superato un requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-254">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="3331c-255">Se impostato su `false`, la proprietà [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (disponibile in ASP.NET Core 1,1 e versioni successive) esegue il cortocircuito dell'esecuzione dei gestori quando viene chiamato `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="3331c-255">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="3331c-256">`InvokeHandlersAfterFailure` viene impostato il valore predefinito `true`, nel qual caso vengono chiamati tutti i gestori.</span><span class="sxs-lookup"><span data-stu-id="3331c-256">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="3331c-257">I gestori di autorizzazione vengono chiamati anche se l'autenticazione ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="3331c-257">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="3331c-258">Perché si vogliono usare più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="3331c-258">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="3331c-259">Nei casi in cui si desidera che la valutazione sia basata su **o** , implementare più gestori per un singolo requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-259">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="3331c-260">Ad esempio, Microsoft dispone di porte che si aprono solo con le schede chiave.</span><span class="sxs-lookup"><span data-stu-id="3331c-260">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="3331c-261">Se si lascia la scheda chiave a casa, il receptionist stampa un adesivo temporaneo e apre lo sportello.</span><span class="sxs-lookup"><span data-stu-id="3331c-261">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="3331c-262">In questo scenario si avrà un singolo requisito, *BuildingEntry*, ma più gestori, ognuno di essi esaminando un singolo requisito.</span><span class="sxs-lookup"><span data-stu-id="3331c-262">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="3331c-263">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="3331c-263">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="3331c-264">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="3331c-264">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="3331c-265">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="3331c-265">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="3331c-266">Assicurarsi che entrambi i gestori siano [registrati](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="3331c-266">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="3331c-267">Se uno dei due gestori ha esito positivo quando un criterio valuta il `BuildingEntryRequirement`, la valutazione dei criteri ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3331c-267">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="3331c-268">Uso di un Func per soddisfare un criterio</span><span class="sxs-lookup"><span data-stu-id="3331c-268">Using a func to fulfill a policy</span></span>

<span data-ttu-id="3331c-269">In alcuni casi è possibile che l'evasione di un criterio sia semplice da esprimere nel codice.</span><span class="sxs-lookup"><span data-stu-id="3331c-269">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="3331c-270">È possibile specificare un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il generatore di criteri di `RequireAssertion`.</span><span class="sxs-lookup"><span data-stu-id="3331c-270">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="3331c-271">Ad esempio, il `BadgeEntryHandler` precedente potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="3331c-271">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="3331c-272">Accesso al contesto della richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="3331c-272">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="3331c-273">Il metodo `HandleRequirementAsync` implementato in un gestore autorizzazioni presenta due parametri: un `AuthorizationHandlerContext` e il `TRequirement` che si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="3331c-273">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="3331c-274">I Framework come MVC o Jabbr sono liberi di aggiungere qualsiasi oggetto alla proprietà `Resource` nel `AuthorizationHandlerContext` per passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="3331c-274">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="3331c-275">Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nella proprietà `Resource`.</span><span class="sxs-lookup"><span data-stu-id="3331c-275">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="3331c-276">Questa proprietà consente di accedere a `HttpContext`, `RouteData`e tutto il resto fornito da MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3331c-276">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="3331c-277">L'uso della proprietà `Resource` è specifico del Framework.</span><span class="sxs-lookup"><span data-stu-id="3331c-277">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="3331c-278">L'uso delle informazioni nella proprietà `Resource` limita i criteri di autorizzazione a specifici Framework.</span><span class="sxs-lookup"><span data-stu-id="3331c-278">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="3331c-279">È necessario eseguire il cast della proprietà `Resource` usando la parola chiave `is`, quindi verificare che il cast abbia avuto esito positivo per assicurarsi che il codice non si arresti in modo anomalo con un `InvalidCastException` quando viene eseguito in altri Framework:</span><span class="sxs-lookup"><span data-stu-id="3331c-279">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end