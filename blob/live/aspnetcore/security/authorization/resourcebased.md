---
title: Autorizzazione basata sulle risorse in ASP.NET Core
author: scottaddie
description: Informazioni su come implementare l'autorizzazione basata sulle risorse in un'applicazione ASP.NET di base quando un attributo Authorize non sono sufficienti.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 708f306da740870b106cbeeb96879480f8745439
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="50e9b-103">Autorizzazione basata sulle risorse</span><span class="sxs-lookup"><span data-stu-id="50e9b-103">Resource-based authorization</span></span>

<span data-ttu-id="50e9b-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="50e9b-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="50e9b-105">Strategia di autorizzazione dipende dalla risorsa a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="50e9b-105">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="50e9b-106">Si consideri un documento che contiene una proprietà dell'autore.</span><span class="sxs-lookup"><span data-stu-id="50e9b-106">Consider a document which has an author property.</span></span> <span data-ttu-id="50e9b-107">È consentito solo l'autore di aggiornare il documento.</span><span class="sxs-lookup"><span data-stu-id="50e9b-107">Only the author is allowed to update the document.</span></span> <span data-ttu-id="50e9b-108">Di conseguenza, il documento deve essere recuperato dall'archivio dati prima di poter eseguire la valutazione di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="50e9b-108">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="50e9b-109">Attributo valutazione si verifica prima dell'associazione a dati e prima dell'esecuzione del gestore di pagina o azione che carica il documento.</span><span class="sxs-lookup"><span data-stu-id="50e9b-109">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="50e9b-110">Per questi motivi, l'autorizzazione dichiarativa con un `[Authorize]` attributo non sono sufficienti.</span><span class="sxs-lookup"><span data-stu-id="50e9b-110">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="50e9b-111">In alternativa, è possibile richiamare un metodo di autorizzazione personalizzato&mdash;uno stile noto come autorizzazione imperativa.</span><span class="sxs-lookup"><span data-stu-id="50e9b-111">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="50e9b-112">Utilizzare il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) per esplorare le funzionalità descritte in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="50e9b-112">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="50e9b-113">Utilizzare l'autorizzazione imperativo</span><span class="sxs-lookup"><span data-stu-id="50e9b-113">Use imperative authorization</span></span>

<span data-ttu-id="50e9b-114">Autorizzazione viene implementata come un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) del servizio e viene registrato nell'insieme di servizio all'interno di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="50e9b-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="50e9b-115">Il servizio viene reso disponibile tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) a gestori di pagina o azioni.</span><span class="sxs-lookup"><span data-stu-id="50e9b-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="50e9b-116">`IAuthorizationService`dispone di due `AuthorizeAsync` overload del metodo: uno che accetta la risorsa e il nome del criterio e l'altro che accetta la risorsa e un elenco di requisiti per la valutazione.</span><span class="sxs-lookup"><span data-stu-id="50e9b-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50e9b-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50e9b-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="50e9b-119">Nell'esempio seguente, la risorsa da proteggere viene caricata in un oggetto personalizzato `Document` oggetto.</span><span class="sxs-lookup"><span data-stu-id="50e9b-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="50e9b-120">Un `AuthorizeAsync` overload viene richiamato per determinare se l'utente corrente è autorizzato a modificare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="50e9b-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="50e9b-121">Un criterio di autorizzazione personalizzato "EditPolicy" è inserito nella decisione.</span><span class="sxs-lookup"><span data-stu-id="50e9b-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="50e9b-122">Vedere [autorizzazione basata su criteri personalizzati](xref:security/authorization/policies) per ulteriori informazioni sulla creazione di criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="50e9b-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="50e9b-123">Il codice seguente si supponga di esempi è stata eseguita l'autenticazione e un set di `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="50e9b-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50e9b-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50e9b-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="50e9b-126">Scrivere un gestore di risorse</span><span class="sxs-lookup"><span data-stu-id="50e9b-126">Write a resource-based handler</span></span>

<span data-ttu-id="50e9b-127">Scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diverso rispetto a [scrittura di un gestore di requisiti normale](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="50e9b-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="50e9b-128">Creare una classe del requisito personalizzata e implementare una classe del requisito del gestore.</span><span class="sxs-lookup"><span data-stu-id="50e9b-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="50e9b-129">La classe del gestore specifica sia il requisito e il tipo di risorsa.</span><span class="sxs-lookup"><span data-stu-id="50e9b-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="50e9b-130">Ad esempio, un gestore che utilizzano un `SameAuthorRequirement` requisito e un `Document` risorse è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="50e9b-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50e9b-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50e9b-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="50e9b-133">Registrare un gestore in e il requisito di `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="50e9b-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="50e9b-134">Requisiti operativi</span><span class="sxs-lookup"><span data-stu-id="50e9b-134">Operational requirements</span></span>

<span data-ttu-id="50e9b-135">Se si apportano decisioni in base ai risultati delle CRUD (**C**rea, **R**eggere, **U**orna, **D**Elimina) operazioni, utilizzare il [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe helper.</span><span class="sxs-lookup"><span data-stu-id="50e9b-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="50e9b-136">Questa classe consente di scrivere un singolo gestore anziché una singola classe per ogni tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="50e9b-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="50e9b-137">Per utilizzarlo, fornire alcuni nomi di operazione:</span><span class="sxs-lookup"><span data-stu-id="50e9b-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="50e9b-138">Il gestore viene implementato come indicato di seguito, utilizzando un `OperationAuthorizationRequirement` requisito e un `Document` risorse:</span><span class="sxs-lookup"><span data-stu-id="50e9b-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50e9b-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50e9b-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="50e9b-141">Il gestore precedente convalida l'operazione utilizzando la risorsa, l'identità dell'utente e il requisito `Name` proprietà.</span><span class="sxs-lookup"><span data-stu-id="50e9b-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="50e9b-142">Per chiamare un gestore di risorse operative, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore di pagina o azione.</span><span class="sxs-lookup"><span data-stu-id="50e9b-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="50e9b-143">Nell'esempio seguente determina se l'utente autenticato può visualizzare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="50e9b-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="50e9b-144">Il codice seguente si supponga di esempi è stata eseguita l'autenticazione e un set di `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="50e9b-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50e9b-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="50e9b-146">Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="50e9b-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="50e9b-147">Se si verifica un errore di autorizzazione, ma l'utente viene autenticato, restituendo `ForbidResult` informa qualsiasi middleware di autenticazione che l'autorizzazione non riuscita.</span><span class="sxs-lookup"><span data-stu-id="50e9b-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="50e9b-148">Oggetto `ChallengeResult` viene restituito quando l'autenticazione deve essere eseguita.</span><span class="sxs-lookup"><span data-stu-id="50e9b-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="50e9b-149">Per i client del browser interattivo, potrebbe essere opportuno reindirizzare l'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="50e9b-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50e9b-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50e9b-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="50e9b-151">Se l'autorizzazione ha esito positivo, viene restituita la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="50e9b-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="50e9b-152">Se l'autorizzazione ha esito negativo, restituisce `ChallengeResult` indica qualsiasi middleware di autenticazione di autorizzazione non riuscita, il middleware può richiedere la risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="50e9b-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="50e9b-153">Impossibile restituire un codice di stato 401 o 403 una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="50e9b-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="50e9b-154">Per i client del browser interattiva, è possibile che il reindirizzamento dell'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="50e9b-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
