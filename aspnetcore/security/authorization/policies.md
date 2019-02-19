---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e utilizzare i gestori di criteri di autorizzazione per l'applicazione dei requisiti di autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: be4812487c92a16c44e3983b234bc9e31be65190
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410389"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="23555-103">Autorizzazione basata su criteri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23555-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="23555-104">Sotto le quinte [autorizzazione basata sui ruoli](xref:security/authorization/roles) e [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore di requisito e un criterio preconfigurato.</span><span class="sxs-lookup"><span data-stu-id="23555-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="23555-105">Questi blocchi predefiniti supportano l'espressione di valutazioni di autorizzazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="23555-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="23555-106">Il risultato è una struttura di autorizzazione più avanzate, riutilizzabili e testabili.</span><span class="sxs-lookup"><span data-stu-id="23555-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="23555-107">Un criterio di autorizzazione è costituito da uno o più requisiti.</span><span class="sxs-lookup"><span data-stu-id="23555-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="23555-108">In cui è registrata come parte della configurazione del servizio di autorizzazione, il `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="23555-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="23555-109">Nell'esempio precedente, viene creato un criterio di "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="23555-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="23555-110">Ha un unico requisito&mdash;che di una durata minima, che viene fornita come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="23555-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="23555-111">I criteri vengono applicati usando i `[Authorize]` attributo con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="23555-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="23555-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23555-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="23555-113">Requisiti</span><span class="sxs-lookup"><span data-stu-id="23555-113">Requirements</span></span>

<span data-ttu-id="23555-114">Un requisito di autorizzazione è una raccolta di parametri di dati che i criteri possono utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="23555-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="23555-115">Nei nostri criteri "AtLeast21", il requisito è un singolo parametro&mdash;la validità minima.</span><span class="sxs-lookup"><span data-stu-id="23555-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="23555-116">Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuoto.</span><span class="sxs-lookup"><span data-stu-id="23555-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="23555-117">Un requisito di validità minima con parametri potrebbe essere implementato come segue:</span><span class="sxs-lookup"><span data-stu-id="23555-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="23555-118">Se un criterio di autorizzazione contiene più i requisiti di autorizzazione, è necessario passare tutti i requisiti in ordine per la valutazione dei criteri abbia esito positivo.</span><span class="sxs-lookup"><span data-stu-id="23555-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="23555-119">In altre parole, più i requisiti di autorizzazione aggiunti a un singolo criterio di autorizzazione vengono considerati in un **AND** base.</span><span class="sxs-lookup"><span data-stu-id="23555-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="23555-120">Un requisito non deve necessariamente avere dei dati o le proprietà.</span><span class="sxs-lookup"><span data-stu-id="23555-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="23555-121">Gestori di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="23555-121">Authorization handlers</span></span>

<span data-ttu-id="23555-122">Un gestore di autorizzazione è responsabile per la valutazione delle proprietà del requisito.</span><span class="sxs-lookup"><span data-stu-id="23555-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="23555-123">Il gestore autorizzazioni valuta i requisiti per un oggetto fornito [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) per determinare se l'accesso è consentito.</span><span class="sxs-lookup"><span data-stu-id="23555-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="23555-124">Può avere un requisito [più gestori](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="23555-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="23555-125">Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito per essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="23555-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="23555-126">In alternativa, è possibile implementare un gestore [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.</span><span class="sxs-lookup"><span data-stu-id="23555-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="23555-127">Usare un gestore per uno dei requisiti</span><span class="sxs-lookup"><span data-stu-id="23555-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="23555-128">Di seguito è riportato un esempio di una relazione uno a uno in cui un gestore della durata minima utilizza un unico requisito:</span><span class="sxs-lookup"><span data-stu-id="23555-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="23555-129">Il codice precedente determina se l'entità utente corrente ha una data di nascita di attestazione che sia stato emesso da un'autorità emittente conosciuta e attendibile.</span><span class="sxs-lookup"><span data-stu-id="23555-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="23555-130">Autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata.</span><span class="sxs-lookup"><span data-stu-id="23555-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="23555-131">Quando un'attestazione è presente, viene calcolato l'età dell'utente.</span><span class="sxs-lookup"><span data-stu-id="23555-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="23555-132">Se l'utente soddisfa il numero minimo giorni definito dai requisiti, autorizzazione venga considerata riuscita.</span><span class="sxs-lookup"><span data-stu-id="23555-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="23555-133">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.</span><span class="sxs-lookup"><span data-stu-id="23555-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="23555-134">Usare un gestore per i requisiti di più</span><span class="sxs-lookup"><span data-stu-id="23555-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="23555-135">Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazione possa gestire tre diversi tipi di requisiti:</span><span class="sxs-lookup"><span data-stu-id="23555-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="23555-136">Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene i requisiti non contrassegnata come positiva.</span><span class="sxs-lookup"><span data-stu-id="23555-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="23555-137">Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="23555-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="23555-138">Nel caso di un' `EditPermission` o `DeletePermission` requisito, che potrà deve essere un proprietario per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="23555-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="23555-139">Registrazione del gestore</span><span class="sxs-lookup"><span data-stu-id="23555-139">Handler registration</span></span>

<span data-ttu-id="23555-140">I gestori registrati dell'insieme di servizi durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="23555-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="23555-141">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23555-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="23555-142">Ogni gestore viene aggiunto alla raccolta di servizi richiamando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="23555-142">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="23555-143">Che cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="23555-143">What should a handler return?</span></span>

<span data-ttu-id="23555-144">Si noti che il `Handle` metodo nella [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="23555-144">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="23555-145">Come è stato positivo o negativo indicato?</span><span class="sxs-lookup"><span data-stu-id="23555-145">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="23555-146">Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che non è stata convalidata.</span><span class="sxs-lookup"><span data-stu-id="23555-146">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="23555-147">Un gestore non è necessario gestire gli errori a livello generale, come altri gestori per gli stessi requisiti potrebbero avere esito positivo.</span><span class="sxs-lookup"><span data-stu-id="23555-147">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="23555-148">Per garantire il caso di errore, anche se altri gestori di requisiti abbia esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="23555-148">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="23555-149">Se impostato su `false`, il [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) proprietà (disponibile in ASP.NET Core 1.1 e versioni successive) consente di bloccare l'esecuzione dei gestori quando `context.Fail` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="23555-149">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="23555-150">`InvokeHandlersAfterFailure` Per impostazione predefinita `true`, nel qual caso tutti i gestori vengono chiamati.</span><span class="sxs-lookup"><span data-stu-id="23555-150">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="23555-151">In questo modo i requisiti per produrre effetti collaterali, ad esempio la registrazione, che hanno sempre luogo anche se `context.Fail` è stato chiamato in un altro gestore.</span><span class="sxs-lookup"><span data-stu-id="23555-151">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="23555-152">Il motivo per cui è consigliabile più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="23555-152">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="23555-153">Nei casi in cui si desidera valutazione in un **o** singoli, implementare più gestori per un unico requisito.</span><span class="sxs-lookup"><span data-stu-id="23555-153">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="23555-154">Ad esempio, Microsoft ha le porte che essere aperto solo con le schede principali.</span><span class="sxs-lookup"><span data-stu-id="23555-154">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="23555-155">Se si lascia la smart card a casa, l'addetto ai ricoveri viene stampato un adesivo temporaneo e apre le porte per l'utente.</span><span class="sxs-lookup"><span data-stu-id="23555-155">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="23555-156">In questo scenario sarebbe necessario un unico requisito *BuildingEntry*, ma più gestori, ciascuno di essi esaminando un unico requisito.</span><span class="sxs-lookup"><span data-stu-id="23555-156">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="23555-157">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="23555-157">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="23555-158">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="23555-158">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="23555-159">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="23555-159">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="23555-160">Assicurarsi che entrambi i gestori [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="23555-160">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="23555-161">Se ha esito positivo su gestore quando un criterio valuta le `BuildingEntryRequirement`, ha esito positivo della valutazione dei criteri.</span><span class="sxs-lookup"><span data-stu-id="23555-161">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="23555-162">Usando una funzione per soddisfare un criterio</span><span class="sxs-lookup"><span data-stu-id="23555-162">Using a func to fulfill a policy</span></span>

<span data-ttu-id="23555-163">Si possono verificare situazioni in cui che soddisfano un criterio è semplice da esprimere nel codice.</span><span class="sxs-lookup"><span data-stu-id="23555-163">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="23555-164">È possibile fornire un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.</span><span class="sxs-lookup"><span data-stu-id="23555-164">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="23555-165">Ad esempio, il precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="23555-165">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="23555-166">Accesso al contesto di richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="23555-166">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="23555-167">Il `HandleRequirementAsync` metodo si implementa in un gestore di autorizzazione ha due parametri: un `AuthorizationHandlerContext` e il `TRequirement` si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="23555-167">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="23555-168">Framework come MVC o Jabbr sono liberi di aggiunta di un oggetto per il `Resource` proprietà il `AuthorizationHandlerContext` per passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="23555-168">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="23555-169">Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nel `Resource` proprietà.</span><span class="sxs-lookup"><span data-stu-id="23555-169">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="23555-170">Questa proprietà consente di accedervi `HttpContext`, `RouteData`e tutto quello che altrimenti fornito da MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="23555-170">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="23555-171">L'uso del `Resource` proprietà è framework specifico.</span><span class="sxs-lookup"><span data-stu-id="23555-171">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="23555-172">Utilizzando le informazioni nel `Resource` proprietà limita i criteri di autorizzazione al framework specifico.</span><span class="sxs-lookup"><span data-stu-id="23555-172">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="23555-173">È necessario eseguire il cast di `Resource` proprietà usando la `is` (parola chiave) e quindi confermare il cast ha avuto esito positivo per assicurarsi che il codice non di arresto anomalo con un `InvalidCastException` quando eseguono in altri Framework:</span><span class="sxs-lookup"><span data-stu-id="23555-173">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
