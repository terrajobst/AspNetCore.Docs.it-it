---
title: Autorizzazione personalizzata basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e utilizzare i gestori di criteri di autorizzazione personalizzato per applicare i requisiti di autorizzazione in un'applicazione ASP.NET Core.
keywords: ASP.NET Core, autorizzazione, i criteri personalizzati, i criteri di autorizzazione
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="8a289-104">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="8a289-104">Custom policy-based authorization</span></span>

<span data-ttu-id="8a289-105">Nel sistema il [autorizzazione basata sui ruoli](xref:security/authorization/roles) e [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore del requisito e un criterio configurato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="8a289-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="8a289-106">Questi blocchi predefiniti supportano l'espressione di valutazioni di autorizzazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="8a289-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="8a289-107">Il risultato è una struttura di autorizzazioni complete, riutilizzabili e testabili.</span><span class="sxs-lookup"><span data-stu-id="8a289-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="8a289-108">Un criterio di autorizzazione è costituito da uno o più requisiti.</span><span class="sxs-lookup"><span data-stu-id="8a289-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="8a289-109">È registrato come parte della configurazione del servizio di autorizzazione, nel `ConfigureServices` metodo la `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="8a289-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="8a289-110">Nell'esempio precedente, viene creato un criterio di "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="8a289-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="8a289-111">Ha un requisito singolo, che di una durata minima, che viene fornito come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="8a289-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="8a289-112">I criteri vengono applicati tramite il `[Authorize]` attributo con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="8a289-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="8a289-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8a289-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="8a289-114">Requisiti</span><span class="sxs-lookup"><span data-stu-id="8a289-114">Requirements</span></span>

<span data-ttu-id="8a289-115">Un requisito di autorizzazione è una raccolta di parametri dei dati che un criterio è possibile utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="8a289-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="8a289-116">In questo criterio "AtLeast21", il requisito è un singolo parametro&mdash;la data minima.</span><span class="sxs-lookup"><span data-stu-id="8a289-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="8a289-117">Implementa un requisito `IAuthorizationRequirement`, che è un'interfaccia marcatore vuoto.</span><span class="sxs-lookup"><span data-stu-id="8a289-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="8a289-118">Un requisito di età minima con parametri potrebbe essere implementato come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="8a289-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="8a289-119">Non è un requisito a dati o proprietà.</span><span class="sxs-lookup"><span data-stu-id="8a289-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="8a289-120">Gestori di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="8a289-120">Authorization handlers</span></span>

<span data-ttu-id="8a289-121">Un gestore di autorizzazione è responsabile per la valutazione delle proprietà di un requisito.</span><span class="sxs-lookup"><span data-stu-id="8a289-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="8a289-122">Il gestore autorizzazioni valuta i requisiti in un oggetto fornito `AuthorizationHandlerContext` per determinare se è consentito l'accesso.</span><span class="sxs-lookup"><span data-stu-id="8a289-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="8a289-123">Può avere un requisito [più gestori](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="8a289-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="8a289-124">I gestori di ereditano `AuthorizationHandler<T>`, dove `T` è il requisito deve essere gestito.</span><span class="sxs-lookup"><span data-stu-id="8a289-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="8a289-125">Il gestore di età minima potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8a289-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="8a289-126">Il codice precedente determina se l'entità utente corrente dispone di una data di nascita di attestazioni che è stata eseguita da un'autorità emittente attendibile e nota.</span><span class="sxs-lookup"><span data-stu-id="8a289-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="8a289-127">Autorizzazione non può verificarsi quando la richiesta è manca, nel qual caso viene restituita un'attività completata.</span><span class="sxs-lookup"><span data-stu-id="8a289-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="8a289-128">Quando un'attestazione è presente, viene calcolato l'età dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8a289-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="8a289-129">Se l'utente soddisfi il periodo di memorizzazione minimo definito dai requisiti, autorizzazione viene considerata riuscita.</span><span class="sxs-lookup"><span data-stu-id="8a289-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="8a289-130">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come parametro.</span><span class="sxs-lookup"><span data-stu-id="8a289-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="8a289-131">Registrazione del gestore</span><span class="sxs-lookup"><span data-stu-id="8a289-131">Handler registration</span></span>

<span data-ttu-id="8a289-132">Gestori eventi vengono registrati nella raccolta di servizi durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="8a289-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="8a289-133">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8a289-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="8a289-134">Ogni gestore viene aggiunto alla raccolta di servizi richiamando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="8a289-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="8a289-135">Cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="8a289-135">What should a handler return?</span></span>

<span data-ttu-id="8a289-136">Si noti che il `Handle` metodo il [esempio gestore](#security-authorization-handler-example) non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="8a289-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="8a289-137">Come viene riportato lo stato di esito positivo o un errore indicato?</span><span class="sxs-lookup"><span data-stu-id="8a289-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="8a289-138">Indica l'esito positivo chiamando un gestore `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stata convalidata.</span><span class="sxs-lookup"><span data-stu-id="8a289-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="8a289-139">Un gestore non è necessario gestire gli errori in genere, come altri gestori per lo stesso requisito può essere completata.</span><span class="sxs-lookup"><span data-stu-id="8a289-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="8a289-140">Errore, anche se altri gestori di requisiti di esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="8a289-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="8a289-141">Indipendentemente dal fatto ciò che è definito all'interno del gestore, tutti i gestori per un requisito verranno chiamati quando un criterio richiede il requisito.</span><span class="sxs-lookup"><span data-stu-id="8a289-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="8a289-142">In questo modo i requisiti di effetti collaterali, ad esempio la registrazione, che verrà sempre eseguita anche se `context.Fail()` è stato chiamato in un altro gestore.</span><span class="sxs-lookup"><span data-stu-id="8a289-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="8a289-143">Perché desiderare più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="8a289-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="8a289-144">Nei casi in cui si desidera valutazione in un **o** base, implementare più gestori per un unico requisito.</span><span class="sxs-lookup"><span data-stu-id="8a289-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="8a289-145">Microsoft ha ad esempio, le porte che viene aperta solo con schede di chiave.</span><span class="sxs-lookup"><span data-stu-id="8a289-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="8a289-146">Se si lascia la smart card a casa, il centralinista Stampa etichetta della custodia temporanea e verrà aperta la porta.</span><span class="sxs-lookup"><span data-stu-id="8a289-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="8a289-147">In questo scenario, è necessario un unico requisito *BuildingEntry*, ma più gestori, ciascuno di essi un unico requisito di analisi.</span><span class="sxs-lookup"><span data-stu-id="8a289-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="8a289-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="8a289-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="8a289-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="8a289-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="8a289-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="8a289-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="8a289-151">Assicurarsi che entrambi i gestori siano [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="8a289-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="8a289-152">Valuta se ha esito positivo su gestore, quando un criterio di `BuildingEntryRequirement`, ha esito positivo della valutazione dei criteri.</span><span class="sxs-lookup"><span data-stu-id="8a289-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="8a289-153">Utilizzo di func per soddisfare i criteri di</span><span class="sxs-lookup"><span data-stu-id="8a289-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="8a289-154">Potrebbero verificarsi situazioni in cui che soddisfano un criterio è semplice da express nel codice.</span><span class="sxs-lookup"><span data-stu-id="8a289-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="8a289-155">È possibile fornire un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.</span><span class="sxs-lookup"><span data-stu-id="8a289-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="8a289-156">Ad esempio, il precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="8a289-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="8a289-157">Accesso al contesto di richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="8a289-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="8a289-158">Il `HandleRequirementAsync` metodo è implementato in un gestore di autorizzazione presenta due parametri: un `AuthorizationHandlerContext` e `TRequirement` si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="8a289-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="8a289-159">Framework di MVC o Jabbr hanno la possibilità di aggiungere qualsiasi oggetto di `Resource` proprietà il `AuthorizationHandlerContext` per passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="8a289-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="8a289-160">Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nel `Resource` proprietà.</span><span class="sxs-lookup"><span data-stu-id="8a289-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="8a289-161">Questa proprietà fornisce l'accesso a `HttpContext`, `RouteData`e tutto ciò che altrimenti forniti da MVC e pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="8a289-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="8a289-162">L'utilizzo del `Resource` è di proprietà specifici del framework.</span><span class="sxs-lookup"><span data-stu-id="8a289-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="8a289-163">Utilizzando le informazioni nel `Resource` proprietà limita i criteri di autorizzazione per un framework specifico.</span><span class="sxs-lookup"><span data-stu-id="8a289-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="8a289-164">È necessario eseguire il cast di `Resource` proprietà utilizzando il `as` (parola chiave) e quindi confermare il cast ha esito positivo per verificare che il codice non di arresto anomalo con un `InvalidCastException` quando viene eseguito da altri Framework:</span><span class="sxs-lookup"><span data-stu-id="8a289-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
