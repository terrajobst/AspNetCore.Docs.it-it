---
title: Autorizzazione basata sulle risorse in ASP.NET Core
author: scottaddie
description: Informazioni su come implementare l'autorizzazione basata sulle risorse in un'app ASP.NET Core quando un attributo di autorizzazione non è sufficiente.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 835592521c714e270595e1448ae6e0aed1707b77
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590010"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="dd03e-103">Autorizzazione basata sulle risorse in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd03e-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="dd03e-104">La strategia di autorizzazione dipende dalla risorsa a cui si accede.</span><span class="sxs-lookup"><span data-stu-id="dd03e-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="dd03e-105">Si consideri un documento con una proprietà Author.</span><span class="sxs-lookup"><span data-stu-id="dd03e-105">Consider a document that has an author property.</span></span> <span data-ttu-id="dd03e-106">Solo l'autore è autorizzato ad aggiornare il documento.</span><span class="sxs-lookup"><span data-stu-id="dd03e-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="dd03e-107">Di conseguenza, il documento deve essere recuperato dall'archivio dati prima che possa essere eseguita la valutazione dell'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="dd03e-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="dd03e-108">La valutazione degli attributi viene eseguita prima data binding e prima dell'esecuzione del gestore di pagina o dell'azione che carica il documento.</span><span class="sxs-lookup"><span data-stu-id="dd03e-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="dd03e-109">Per questi motivi, l'autorizzazione dichiarativa con un attributo `[Authorize]` non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="dd03e-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="dd03e-110">È invece possibile richiamare un metodo di autorizzazione personalizzato &mdash;a stile definito *autorizzazione imperativa*.</span><span class="sxs-lookup"><span data-stu-id="dd03e-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="dd03e-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="dd03e-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="dd03e-112">[Creare un'app ASP.NET Core con i dati utente protetti dall'autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che usa l'autorizzazione basata sulle risorse.</span><span class="sxs-lookup"><span data-stu-id="dd03e-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="dd03e-113">USA autorizzazione imperativa</span><span class="sxs-lookup"><span data-stu-id="dd03e-113">Use imperative authorization</span></span>

<span data-ttu-id="dd03e-114">L'autorizzazione viene implementata come servizio [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) e viene registrata nella raccolta di servizi all'interno della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="dd03e-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="dd03e-115">Il servizio viene reso disponibile tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) in gestori di pagine o azioni.</span><span class="sxs-lookup"><span data-stu-id="dd03e-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="dd03e-116">`IAuthorizationService` dispone di due overload del metodo `AuthorizeAsync`: uno accetta la risorsa e il nome del criterio e l'altro accetta la risorsa e un elenco di requisiti da valutare.</span><span class="sxs-lookup"><span data-stu-id="dd03e-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="dd03e-117">Nell'esempio seguente la risorsa da proteggere viene caricata in un oggetto `Document` personalizzato.</span><span class="sxs-lookup"><span data-stu-id="dd03e-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="dd03e-118">Viene richiamato un overload `AuthorizeAsync` per determinare se l'utente corrente è autorizzato a modificare il documento fornito.</span><span class="sxs-lookup"><span data-stu-id="dd03e-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="dd03e-119">I criteri di autorizzazione "EditPolicy" personalizzati vengono presi in considerazione nella decisione.</span><span class="sxs-lookup"><span data-stu-id="dd03e-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="dd03e-120">Per altre informazioni sulla creazione di criteri di autorizzazione, vedere [autorizzazione personalizzata basata su criteri](xref:security/authorization/policies) .</span><span class="sxs-lookup"><span data-stu-id="dd03e-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="dd03e-121">Negli esempi di codice seguenti si presuppone che l'autenticazione sia stata eseguita e che sia stata impostata la proprietà `User`.</span><span class="sxs-lookup"><span data-stu-id="dd03e-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="dd03e-122">Scrivere un gestore basato su risorse</span><span class="sxs-lookup"><span data-stu-id="dd03e-122">Write a resource-based handler</span></span>

<span data-ttu-id="dd03e-123">La scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diversa dalla [scrittura di un gestore di requisiti semplici](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="dd03e-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="dd03e-124">Creare una classe di requisiti personalizzata e implementare una classe del gestore di requisiti.</span><span class="sxs-lookup"><span data-stu-id="dd03e-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="dd03e-125">Per ulteriori informazioni sulla creazione di una classe requirement, vedere [requirements](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="dd03e-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="dd03e-126">La classe handler specifica sia il requisito che il tipo di risorsa.</span><span class="sxs-lookup"><span data-stu-id="dd03e-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="dd03e-127">Ad esempio, un gestore che utilizza un `SameAuthorRequirement` e una risorsa `Document` segue:</span><span class="sxs-lookup"><span data-stu-id="dd03e-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="dd03e-128">Nell'esempio precedente, si supponga che `SameAuthorRequirement` sia un caso speciale di una classe `SpecificAuthorRequirement` più generica.</span><span class="sxs-lookup"><span data-stu-id="dd03e-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="dd03e-129">La classe `SpecificAuthorRequirement` (non mostrata) contiene una proprietà `Name` che rappresenta il nome dell'autore.</span><span class="sxs-lookup"><span data-stu-id="dd03e-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="dd03e-130">È possibile impostare la proprietà `Name` sull'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="dd03e-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="dd03e-131">Registrare il requisito e il gestore in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="dd03e-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="dd03e-132">Requisiti operativi</span><span class="sxs-lookup"><span data-stu-id="dd03e-132">Operational requirements</span></span>

<span data-ttu-id="dd03e-133">Se si stanno prendendo decisioni in base ai risultati delle operazioni CRUD (create, Read, Update, Delete), usare la classe helper [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) .</span><span class="sxs-lookup"><span data-stu-id="dd03e-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="dd03e-134">Questa classe consente di scrivere un singolo gestore anziché una singola classe per ogni tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="dd03e-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="dd03e-135">Per usarlo, specificare alcuni nomi di operazione:</span><span class="sxs-lookup"><span data-stu-id="dd03e-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="dd03e-136">Il gestore viene implementato come segue, usando un requisito `OperationAuthorizationRequirement` e una risorsa `Document`:</span><span class="sxs-lookup"><span data-stu-id="dd03e-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="dd03e-137">Il gestore precedente convalida l'operazione usando la risorsa, l'identità dell'utente e la proprietà `Name` del requisito.</span><span class="sxs-lookup"><span data-stu-id="dd03e-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="dd03e-138">Verifica e Impedisci un gestore di risorse operativo</span><span class="sxs-lookup"><span data-stu-id="dd03e-138">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="dd03e-139">In questa sezione viene illustrato il modo in cui vengono elaborati i risultati dell'azione di richiesta e proibisce e il modo in cui le</span><span class="sxs-lookup"><span data-stu-id="dd03e-139">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="dd03e-140">Per chiamare un gestore di risorse operative, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore o nell'azione della pagina.</span><span class="sxs-lookup"><span data-stu-id="dd03e-140">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="dd03e-141">Nell'esempio seguente viene determinato se l'utente autenticato è autorizzato a visualizzare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="dd03e-141">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="dd03e-142">Negli esempi di codice seguenti si presuppone che l'autenticazione sia stata eseguita e che sia stata impostata la proprietà `User`.</span><span class="sxs-lookup"><span data-stu-id="dd03e-142">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="dd03e-143">Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="dd03e-143">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="dd03e-144">Se l'autorizzazione ha esito negativo, ma l'utente viene autenticato, restituendo `ForbidResult` informa qualsiasi middleware di autenticazione che l'autorizzazione non è riuscita.</span><span class="sxs-lookup"><span data-stu-id="dd03e-144">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="dd03e-145">Quando è necessario eseguire l'autenticazione, viene restituito un `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="dd03e-145">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="dd03e-146">Per i client del browser interattivo, potrebbe essere opportuno reindirizzare l'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="dd03e-146">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="dd03e-147">Se l'autorizzazione ha esito positivo, viene restituita la vista per il documento.</span><span class="sxs-lookup"><span data-stu-id="dd03e-147">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="dd03e-148">Se l'autorizzazione ha esito negativo, restituendo `ChallengeResult` informa il middleware di autenticazione che l'autorizzazione non è riuscita e il middleware può prendere la risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="dd03e-148">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="dd03e-149">Una risposta appropriata potrebbe restituire un codice di stato 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="dd03e-149">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="dd03e-150">Per i client del browser interattivo, potrebbe significare il reindirizzamento dell'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="dd03e-150">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
