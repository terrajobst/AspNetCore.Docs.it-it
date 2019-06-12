---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e utilizzare i gestori di criteri di autorizzazione per l'applicazione dei requisiti di autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837347"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="c1450-103">Autorizzazione basata su criteri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1450-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c1450-104">Sotto le quinte [autorizzazione basata sui ruoli](xref:security/authorization/roles) e [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore di requisito e un criterio preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="c1450-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="c1450-105">Questi blocchi predefiniti supportano l'espressione di valutazioni di autorizzazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="c1450-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="c1450-106">Il risultato è una struttura di autorizzazione più avanzate, riutilizzabili e testabili.</span><span class="sxs-lookup"><span data-stu-id="c1450-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="c1450-107">Un criterio di autorizzazione è costituito da uno o più requisiti.</span><span class="sxs-lookup"><span data-stu-id="c1450-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="c1450-108">In cui è registrata come parte della configurazione del servizio di autorizzazione, il `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="c1450-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="c1450-109">Nell'esempio precedente, viene creato un criterio di "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="c1450-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="c1450-110">Ha un unico requisito&mdash;che di una durata minima, che viene fornita come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="c1450-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="c1450-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="c1450-111">IAuthorizationService</span></span> 

<span data-ttu-id="c1450-112">Il servizio primario che determina se l'autorizzazione ha esito positivo è <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="c1450-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="c1450-113">Il codice precedente evidenzia due metodi per la [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="c1450-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="c1450-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> è un servizio marcatore senza metodi e il meccanismo di rilevamento se l'autorizzazione ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="c1450-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="c1450-115">Ogni <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> è responsabile della verifica se vengono soddisfatti i requisiti:</span><span class="sxs-lookup"><span data-stu-id="c1450-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

<span data-ttu-id="c1450-116">Il <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> classe viene usato il gestore per contrassegnare se sono stati soddisfatti i requisiti:</span><span class="sxs-lookup"><span data-stu-id="c1450-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="c1450-117">Il codice seguente illustra il semplificata e annotato con commenti, l'implementazione del servizio di autorizzazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="c1450-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="c1450-118">Il codice seguente viene illustrato un tipico `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c1450-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="c1450-119">Uso <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something"]` per l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c1450-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something"]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="c1450-120">Applicare criteri ai controller MVC</span><span class="sxs-lookup"><span data-stu-id="c1450-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="c1450-121">Se si usano le pagine Razor, vedere [applicazione di criteri a Razor Pages](#applying-policies-to-razor-pages) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="c1450-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="c1450-122">I criteri vengono applicati ai controller usando il `[Authorize]` attributo con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="c1450-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1450-123">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c1450-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="c1450-124">Applicare criteri a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c1450-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="c1450-125">I criteri vengono applicati a Razor Pages usando il `[Authorize]` attributo con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="c1450-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1450-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c1450-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="c1450-127">Criteri possono essere applicati anche alle pagine Razor con un [convenzione autorizzazione](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="c1450-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="c1450-128">Requisiti</span><span class="sxs-lookup"><span data-stu-id="c1450-128">Requirements</span></span>

<span data-ttu-id="c1450-129">Un requisito di autorizzazione è una raccolta di parametri di dati che i criteri possono utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="c1450-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="c1450-130">Nei nostri criteri "AtLeast21", il requisito è un singolo parametro&mdash;la validità minima.</span><span class="sxs-lookup"><span data-stu-id="c1450-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="c1450-131">Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuoto.</span><span class="sxs-lookup"><span data-stu-id="c1450-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="c1450-132">Un requisito di validità minima con parametri potrebbe essere implementato come segue:</span><span class="sxs-lookup"><span data-stu-id="c1450-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="c1450-133">Se un criterio di autorizzazione contiene più i requisiti di autorizzazione, è necessario passare tutti i requisiti in ordine per la valutazione dei criteri abbia esito positivo.</span><span class="sxs-lookup"><span data-stu-id="c1450-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="c1450-134">In altre parole, più i requisiti di autorizzazione aggiunti a un singolo criterio di autorizzazione vengono considerati in un **AND** base.</span><span class="sxs-lookup"><span data-stu-id="c1450-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="c1450-135">Un requisito non deve necessariamente avere dei dati o le proprietà.</span><span class="sxs-lookup"><span data-stu-id="c1450-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="c1450-136">Gestori di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c1450-136">Authorization handlers</span></span>

<span data-ttu-id="c1450-137">Un gestore di autorizzazione è responsabile per la valutazione delle proprietà del requisito.</span><span class="sxs-lookup"><span data-stu-id="c1450-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="c1450-138">Il gestore autorizzazioni valuta i requisiti per un oggetto fornito [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) per determinare se l'accesso è consentito.</span><span class="sxs-lookup"><span data-stu-id="c1450-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="c1450-139">Può avere un requisito [più gestori](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="c1450-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="c1450-140">Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito per essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="c1450-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="c1450-141">In alternativa, è possibile implementare un gestore [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.</span><span class="sxs-lookup"><span data-stu-id="c1450-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="c1450-142">Usare un gestore per uno dei requisiti</span><span class="sxs-lookup"><span data-stu-id="c1450-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="c1450-143">Di seguito è riportato un esempio di una relazione uno a uno in cui un gestore della durata minima utilizza un unico requisito:</span><span class="sxs-lookup"><span data-stu-id="c1450-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="c1450-144">Il codice precedente determina se l'entità utente corrente ha una data di nascita di attestazione che sia stato emesso da un'autorità emittente conosciuta e attendibile.</span><span class="sxs-lookup"><span data-stu-id="c1450-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="c1450-145">Autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata.</span><span class="sxs-lookup"><span data-stu-id="c1450-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="c1450-146">Quando un'attestazione è presente, viene calcolato l'età dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c1450-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="c1450-147">Se l'utente soddisfa il numero minimo giorni definito dai requisiti, autorizzazione venga considerata riuscita.</span><span class="sxs-lookup"><span data-stu-id="c1450-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="c1450-148">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.</span><span class="sxs-lookup"><span data-stu-id="c1450-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="c1450-149">Usare un gestore per i requisiti di più</span><span class="sxs-lookup"><span data-stu-id="c1450-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="c1450-150">Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazione possa gestire tre diversi tipi di requisiti:</span><span class="sxs-lookup"><span data-stu-id="c1450-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="c1450-151">Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene i requisiti non contrassegnata come positiva.</span><span class="sxs-lookup"><span data-stu-id="c1450-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="c1450-152">Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="c1450-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="c1450-153">Nel caso di un' `EditPermission` o `DeletePermission` requisito, che potrà deve essere un proprietario per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="c1450-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="c1450-154">Registrazione del gestore</span><span class="sxs-lookup"><span data-stu-id="c1450-154">Handler registration</span></span>

<span data-ttu-id="c1450-155">I gestori registrati dell'insieme di servizi durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="c1450-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="c1450-156">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c1450-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="c1450-157">Il codice precedente registra `MinimumAgeHandler` come un singleton richiamando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="c1450-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="c1450-158">Gestori possono essere registrati usando uno dei integrato [durate del servizio](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="c1450-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="c1450-159">Che cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="c1450-159">What should a handler return?</span></span>

<span data-ttu-id="c1450-160">Si noti che il `Handle` metodo nella [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="c1450-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="c1450-161">Come è stato positivo o negativo indicato?</span><span class="sxs-lookup"><span data-stu-id="c1450-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="c1450-162">Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che non è stata convalidata.</span><span class="sxs-lookup"><span data-stu-id="c1450-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="c1450-163">Un gestore non è necessario gestire gli errori a livello generale, come altri gestori per gli stessi requisiti potrebbero avere esito positivo.</span><span class="sxs-lookup"><span data-stu-id="c1450-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="c1450-164">Per garantire il caso di errore, anche se altri gestori di requisiti abbia esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="c1450-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="c1450-165">Se chiama un gestore `context.Succeed` o `context.Fail`, tutti gli altri gestori sono ancora chiamati.</span><span class="sxs-lookup"><span data-stu-id="c1450-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="c1450-166">In questo modo i requisiti per produrre effetti collaterali, ad esempio la registrazione, che viene eseguita anche se un altro gestore ha convalidato correttamente o non riuscita di un requisito.</span><span class="sxs-lookup"><span data-stu-id="c1450-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="c1450-167">Se impostato su `false`, il [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) proprietà (disponibile in ASP.NET Core 1.1 e versioni successive) consente di bloccare l'esecuzione dei gestori quando `context.Fail` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c1450-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="c1450-168">`InvokeHandlersAfterFailure` Per impostazione predefinita `true`, nel qual caso tutti i gestori vengono chiamati.</span><span class="sxs-lookup"><span data-stu-id="c1450-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="c1450-169">Gestori di autorizzazione vengono chiamati anche se l'autenticazione non riesce.</span><span class="sxs-lookup"><span data-stu-id="c1450-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="c1450-170">Il motivo per cui è consigliabile più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="c1450-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="c1450-171">Nei casi in cui si desidera valutazione in un **o** singoli, implementare più gestori per un unico requisito.</span><span class="sxs-lookup"><span data-stu-id="c1450-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="c1450-172">Ad esempio, Microsoft ha le porte che essere aperto solo con le schede principali.</span><span class="sxs-lookup"><span data-stu-id="c1450-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="c1450-173">Se si lascia la smart card a casa, l'addetto ai ricoveri viene stampato un adesivo temporaneo e apre le porte per l'utente.</span><span class="sxs-lookup"><span data-stu-id="c1450-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="c1450-174">In questo scenario sarebbe necessario un unico requisito *BuildingEntry*, ma più gestori, ciascuno di essi esaminando un unico requisito.</span><span class="sxs-lookup"><span data-stu-id="c1450-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="c1450-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="c1450-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="c1450-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1450-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="c1450-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1450-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="c1450-178">Assicurarsi che entrambi i gestori [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="c1450-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="c1450-179">Se ha esito positivo su gestore quando un criterio valuta le `BuildingEntryRequirement`, ha esito positivo della valutazione dei criteri.</span><span class="sxs-lookup"><span data-stu-id="c1450-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="c1450-180">Usando una funzione per soddisfare un criterio</span><span class="sxs-lookup"><span data-stu-id="c1450-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="c1450-181">Si possono verificare situazioni in cui che soddisfano un criterio è semplice da esprimere nel codice.</span><span class="sxs-lookup"><span data-stu-id="c1450-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="c1450-182">È possibile fornire un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.</span><span class="sxs-lookup"><span data-stu-id="c1450-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="c1450-183">Ad esempio, il precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="c1450-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="c1450-184">Accesso al contesto di richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="c1450-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="c1450-185">Il `HandleRequirementAsync` metodo si implementa in un gestore di autorizzazione ha due parametri: un `AuthorizationHandlerContext` e il `TRequirement` si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="c1450-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="c1450-186">Framework come MVC o Jabbr sono liberi di aggiunta di un oggetto per il `Resource` proprietà il `AuthorizationHandlerContext` per passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="c1450-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="c1450-187">Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nel `Resource` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c1450-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="c1450-188">Questa proprietà consente di accedervi `HttpContext`, `RouteData`e tutto quello che altrimenti fornito da MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c1450-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="c1450-189">L'uso del `Resource` proprietà è framework specifico.</span><span class="sxs-lookup"><span data-stu-id="c1450-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="c1450-190">Utilizzando le informazioni nel `Resource` proprietà limita i criteri di autorizzazione al framework specifico.</span><span class="sxs-lookup"><span data-stu-id="c1450-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="c1450-191">È necessario eseguire il cast di `Resource` proprietà usando la `is` (parola chiave) e quindi confermare il cast ha avuto esito positivo per assicurarsi che il codice non di arresto anomalo con un `InvalidCastException` quando eseguono in altri Framework:</span><span class="sxs-lookup"><span data-stu-id="c1450-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
