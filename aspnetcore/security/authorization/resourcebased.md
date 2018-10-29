---
title: Autorizzazione basata sulle risorse in ASP.NET Core
author: scottaddie
description: Informazioni su come implementare l'autorizzazione basata sulle risorse in un'app ASP.NET Core quando non sono sufficienti un attributo Authorize.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 2cb3844a38f7482c27fb471343109d51a516ea20
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206696"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="6ac60-103">Autorizzazione basata sulle risorse in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ac60-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="6ac60-104">Strategia di autorizzazione varia a seconda della risorsa a cui si accede.</span><span class="sxs-lookup"><span data-stu-id="6ac60-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="6ac60-105">Si consideri un documento che ha una proprietà dell'autore.</span><span class="sxs-lookup"><span data-stu-id="6ac60-105">Consider a document which has an author property.</span></span> <span data-ttu-id="6ac60-106">Solo l'autore può aggiornare il documento.</span><span class="sxs-lookup"><span data-stu-id="6ac60-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="6ac60-107">Di conseguenza, il documento deve essere recuperato dall'archivio dati prima di poter eseguire la valutazione di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="6ac60-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="6ac60-108">Attributo valutazioni prima del data binding e prima dell'esecuzione del gestore di pagina o l'azione che carica il documento.</span><span class="sxs-lookup"><span data-stu-id="6ac60-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="6ac60-109">Per questi motivi, autorizzazione dichiarative con un `[Authorize]` attributo non sono sufficienti.</span><span class="sxs-lookup"><span data-stu-id="6ac60-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="6ac60-110">In alternativa, è possibile richiamare un metodo di autorizzazione personalizzato&mdash;uno stile noto come autorizzazione imperativo.</span><span class="sxs-lookup"><span data-stu-id="6ac60-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="6ac60-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6ac60-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="6ac60-112">[Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che utilizza l'autorizzazione basata sulle risorse.</span><span class="sxs-lookup"><span data-stu-id="6ac60-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="6ac60-113">Usare l'autorizzazione imperativo</span><span class="sxs-lookup"><span data-stu-id="6ac60-113">Use imperative authorization</span></span>

<span data-ttu-id="6ac60-114">Autorizzazione viene implementato come un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) del servizio e viene registrato nell'insieme del servizio all'interno di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="6ac60-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="6ac60-115">Il servizio viene reso disponibile tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) a gestori di pagina o azioni.</span><span class="sxs-lookup"><span data-stu-id="6ac60-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="6ac60-116">`IAuthorizationService` ha due `AuthorizeAsync` overload dei metodi: uno accetta la risorsa e il nome del criterio e l'altro accetta un elenco di requisiti per la valutazione e la risorsa.</span><span class="sxs-lookup"><span data-stu-id="6ac60-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ac60-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ac60-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="6ac60-119">Nell'esempio seguente, la risorsa deve essere protetto viene caricata in una classe personalizzata `Document` oggetto.</span><span class="sxs-lookup"><span data-stu-id="6ac60-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="6ac60-120">Un `AuthorizeAsync` overload viene richiamato per determinare se l'utente corrente è autorizzato a modificare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="6ac60-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="6ac60-121">Un criterio di autorizzazione "EditPolicy" personalizzato è inserito nella decisione.</span><span class="sxs-lookup"><span data-stu-id="6ac60-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="6ac60-122">Visualizzare [autorizzazione basata sui criteri personalizzati](xref:security/authorization/policies) per altre informazioni sulla creazione di criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="6ac60-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="6ac60-123">Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6ac60-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ac60-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ac60-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="6ac60-126">Scrivere un gestore basata sulle risorse</span><span class="sxs-lookup"><span data-stu-id="6ac60-126">Write a resource-based handler</span></span>

<span data-ttu-id="6ac60-127">Scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diverso da quello [scrivendo un gestore di requisiti semplici](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="6ac60-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="6ac60-128">Creare una classe del requisito personalizzato e implementare una classe del requisito del gestore.</span><span class="sxs-lookup"><span data-stu-id="6ac60-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="6ac60-129">La classe del gestore consente di specificare sia il requisito e il tipo di risorsa.</span><span class="sxs-lookup"><span data-stu-id="6ac60-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="6ac60-130">Ad esempio, un gestore che usano un `SameAuthorRequirement` requisito e un `Document` risorse è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6ac60-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ac60-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ac60-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="6ac60-133">Registrare un gestore in e il requisito di `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="6ac60-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="6ac60-134">Requisiti operativi</span><span class="sxs-lookup"><span data-stu-id="6ac60-134">Operational requirements</span></span>

<span data-ttu-id="6ac60-135">Se si stanno apportando decisioni basate sui risultati di operazioni CRUD (Create, Read, Update, Delete), usare il [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe helper.</span><span class="sxs-lookup"><span data-stu-id="6ac60-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="6ac60-136">Questa classe consente di scrivere un singolo gestore invece di una classe distinta per ogni tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="6ac60-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="6ac60-137">Per usarlo, fornire alcuni nomi di operazione:</span><span class="sxs-lookup"><span data-stu-id="6ac60-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="6ac60-138">Il gestore di è implementato come segue, usando un `OperationAuthorizationRequirement` requisito e un `Document` risorsa:</span><span class="sxs-lookup"><span data-stu-id="6ac60-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ac60-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ac60-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="6ac60-141">Il gestore precedente convalida l'operazione usando la risorsa, l'identità dell'utente e il requisito `Name` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6ac60-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="6ac60-142">Per chiamare un gestore di risorse operativa, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore di pagina o nell'azione.</span><span class="sxs-lookup"><span data-stu-id="6ac60-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="6ac60-143">Nell'esempio seguente determina se l'utente autenticato ha la possibilità di visualizzare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="6ac60-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="6ac60-144">Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6ac60-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6ac60-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="6ac60-146">Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="6ac60-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="6ac60-147">Se si verifica un errore di autorizzazione, ma l'utente viene autenticato, restituendo `ForbidResult` indica a qualsiasi middleware di autenticazione che autorizzazione non riuscita.</span><span class="sxs-lookup"><span data-stu-id="6ac60-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="6ac60-148">Oggetto `ChallengeResult` viene restituito quando l'autenticazione deve essere eseguita.</span><span class="sxs-lookup"><span data-stu-id="6ac60-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="6ac60-149">Per i client browser interattiva, potrebbe essere appropriata reindirizzare l'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="6ac60-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6ac60-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6ac60-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="6ac60-151">Se l'autorizzazione ha esito positivo, viene restituita la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="6ac60-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="6ac60-152">Se l'autorizzazione ha esito negativo, restituendo `ChallengeResult` informa qualsiasi middleware di autenticazione che autorizzazione non riuscita e che il middleware può richiedere la risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="6ac60-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="6ac60-153">Una risposta appropriata potrebbe restituire un codice di stato 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="6ac60-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="6ac60-154">Per i client browser interattiva, è possibile reindirizzare l'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="6ac60-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
