---
title: Autorizzazione basata sulle risorse in ASP.NET Core
author: scottaddie
description: Informazioni su come implementare l'autorizzazione basata sulle risorse in un'app ASP.NET Core quando non sono sufficienti un attributo Authorize.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a163caa26b277fbee6b9d61f8f1d16a60c75b03
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818369"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="c35a4-103">Autorizzazione basata sulle risorse in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c35a4-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c35a4-104">Strategia di autorizzazione varia a seconda della risorsa a cui si accede.</span><span class="sxs-lookup"><span data-stu-id="c35a4-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="c35a4-105">Si consideri un documento contenente una proprietà dell'autore.</span><span class="sxs-lookup"><span data-stu-id="c35a4-105">Consider a document that has an author property.</span></span> <span data-ttu-id="c35a4-106">Solo l'autore può aggiornare il documento.</span><span class="sxs-lookup"><span data-stu-id="c35a4-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="c35a4-107">Di conseguenza, il documento deve essere recuperato dall'archivio dati prima di poter eseguire la valutazione di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c35a4-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="c35a4-108">Attributo valutazioni prima del data binding e prima dell'esecuzione del gestore di pagina o azione che carica il documento.</span><span class="sxs-lookup"><span data-stu-id="c35a4-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="c35a4-109">Per questi motivi, autorizzazione dichiarative con un `[Authorize]` attributo non sono sufficienti.</span><span class="sxs-lookup"><span data-stu-id="c35a4-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="c35a4-110">In alternativa, è possibile richiamare un metodo di autorizzazione personalizzato&mdash;uno stile detto *autorizzazione imperativo*.</span><span class="sxs-lookup"><span data-stu-id="c35a4-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="c35a4-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c35a4-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c35a4-112">[Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che utilizza l'autorizzazione basata sulle risorse.</span><span class="sxs-lookup"><span data-stu-id="c35a4-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="c35a4-113">Usare l'autorizzazione imperativo</span><span class="sxs-lookup"><span data-stu-id="c35a4-113">Use imperative authorization</span></span>

<span data-ttu-id="c35a4-114">Autorizzazione viene implementato come un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) del servizio e viene registrato nell'insieme del servizio all'interno di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="c35a4-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="c35a4-115">Il servizio viene reso disponibile tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) a gestori di pagina o azioni.</span><span class="sxs-lookup"><span data-stu-id="c35a4-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="c35a4-116">`IAuthorizationService` ha due `AuthorizeAsync` overload dei metodi: uno accetta la risorsa e il nome del criterio e l'altro accetta un elenco di requisiti per la valutazione e la risorsa.</span><span class="sxs-lookup"><span data-stu-id="c35a4-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="c35a4-117">Nell'esempio seguente, la risorsa deve essere protetto viene caricata in una classe personalizzata `Document` oggetto.</span><span class="sxs-lookup"><span data-stu-id="c35a4-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="c35a4-118">Un `AuthorizeAsync` overload viene richiamato per determinare se l'utente corrente è autorizzato a modificare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="c35a4-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="c35a4-119">Un criterio di autorizzazione "EditPolicy" personalizzato è inserito nella decisione.</span><span class="sxs-lookup"><span data-stu-id="c35a4-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="c35a4-120">Visualizzare [autorizzazione basata sui criteri personalizzati](xref:security/authorization/policies) per altre informazioni sulla creazione di criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c35a4-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="c35a4-121">Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c35a4-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="c35a4-122">Scrivere un gestore basata sulle risorse</span><span class="sxs-lookup"><span data-stu-id="c35a4-122">Write a resource-based handler</span></span>

<span data-ttu-id="c35a4-123">Scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diverso da quello [scrivendo un gestore di requisiti semplici](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="c35a4-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="c35a4-124">Creare una classe del requisito personalizzato e implementare una classe del requisito del gestore.</span><span class="sxs-lookup"><span data-stu-id="c35a4-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="c35a4-125">Per altre informazioni sulla creazione di una classe del requisito, vedere [requisiti](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="c35a4-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="c35a4-126">La classe del gestore consente di specificare sia il requisito e il tipo di risorsa.</span><span class="sxs-lookup"><span data-stu-id="c35a4-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="c35a4-127">Ad esempio, un gestore che usano un `SameAuthorRequirement` e un `Document` risorse indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c35a4-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="c35a4-128">Nell'esempio precedente, si supponga `SameAuthorRequirement` è un caso speciale di un oggetto generico più `SpecificAuthorRequirement` classe.</span><span class="sxs-lookup"><span data-stu-id="c35a4-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="c35a4-129">Il `SpecificAuthorRequirement` classe (non mostrato) contiene un `Name` proprietà che rappresenta il nome dell'autore.</span><span class="sxs-lookup"><span data-stu-id="c35a4-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="c35a4-130">Il `Name` proprietà può essere impostata per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="c35a4-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="c35a4-131">Registrare il requisito e il gestore in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c35a4-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="c35a4-132">Requisiti operativi</span><span class="sxs-lookup"><span data-stu-id="c35a4-132">Operational requirements</span></span>

<span data-ttu-id="c35a4-133">Se si stanno apportando decisioni basate sui risultati di operazioni CRUD (Create, Read, Update, Delete), usare il [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe helper.</span><span class="sxs-lookup"><span data-stu-id="c35a4-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="c35a4-134">Questa classe consente di scrivere un singolo gestore invece di una classe distinta per ogni tipo di operazione.</span><span class="sxs-lookup"><span data-stu-id="c35a4-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="c35a4-135">Per usarlo, fornire alcuni nomi di operazione:</span><span class="sxs-lookup"><span data-stu-id="c35a4-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="c35a4-136">Il gestore di è implementato come segue, usando un `OperationAuthorizationRequirement` requisito e un `Document` risorsa:</span><span class="sxs-lookup"><span data-stu-id="c35a4-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="c35a4-137">Il gestore precedente convalida l'operazione usando la risorsa, l'identità dell'utente e il requisito `Name` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c35a4-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="c35a4-138">Per chiamare un gestore di risorse operativa, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore di pagina o nell'azione.</span><span class="sxs-lookup"><span data-stu-id="c35a4-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="c35a4-139">Nell'esempio seguente determina se l'utente autenticato ha la possibilità di visualizzare il documento specificato.</span><span class="sxs-lookup"><span data-stu-id="c35a4-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="c35a4-140">Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c35a4-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="c35a4-141">Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="c35a4-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="c35a4-142">Se si verifica un errore di autorizzazione, ma l'utente viene autenticato, restituendo `ForbidResult` indica a qualsiasi middleware di autenticazione che autorizzazione non riuscita.</span><span class="sxs-lookup"><span data-stu-id="c35a4-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="c35a4-143">Oggetto `ChallengeResult` viene restituito quando l'autenticazione deve essere eseguita.</span><span class="sxs-lookup"><span data-stu-id="c35a4-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="c35a4-144">Per i client browser interattiva, potrebbe essere appropriata reindirizzare l'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="c35a4-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="c35a4-145">Se l'autorizzazione ha esito positivo, viene restituita la visualizzazione del documento.</span><span class="sxs-lookup"><span data-stu-id="c35a4-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="c35a4-146">Se l'autorizzazione ha esito negativo, restituendo `ChallengeResult` informa qualsiasi middleware di autenticazione che autorizzazione non riuscita e che il middleware può richiedere la risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="c35a4-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="c35a4-147">Una risposta appropriata potrebbe restituire un codice di stato 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="c35a4-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="c35a4-148">Per i client browser interattiva, è possibile reindirizzare l'utente a una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="c35a4-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
