---
title: Autorizzazione basata sulle risorse
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: d3575619c53e77dadc293ea2bb7dc72501a8a1e3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="ed85d-103">Autorizzazione basata sulle risorse</span><span class="sxs-lookup"><span data-stu-id="ed85d-103">Resource Based Authorization</span></span>

<a name="security-authorization-resource-based"></a>

<span data-ttu-id="ed85d-104">Autorizzazione spesso dipende dalla risorsa a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="ed85d-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="ed85d-105">Ad esempio, un documento può contenere una proprietà author.</span><span class="sxs-lookup"><span data-stu-id="ed85d-105">For example, a document may have an author property.</span></span> <span data-ttu-id="ed85d-106">Solo l'autore del documento sarebbe possibile aggiornarlo, pertanto la risorsa deve essere caricata dal repository di documenti prima di effettuare una valutazione di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ed85d-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="ed85d-107">Questa operazione non può essere eseguita con l'attributo Authorize come attributo valutazione ha luogo prima del data binding e prima dell'esecuzione di codice per caricare una risorsa all'interno di un'azione.</span><span class="sxs-lookup"><span data-stu-id="ed85d-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="ed85d-108">Invece di autorizzazione dichiarativa, il metodo di attributo, è necessario utilizzare l'autorizzazione imperativa, in cui uno sviluppatore chiama una funzione di authorize interno del codice.</span><span class="sxs-lookup"><span data-stu-id="ed85d-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="ed85d-109">Autorizzazione all'interno del codice</span><span class="sxs-lookup"><span data-stu-id="ed85d-109">Authorizing within your code</span></span>

<span data-ttu-id="ed85d-110">Autorizzazione viene implementata come un servizio, `IAuthorizationService`, registrato nell'insieme di servizio e disponibile tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) per i controller per accedere.</span><span class="sxs-lookup"><span data-stu-id="ed85d-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

<span data-ttu-id="ed85d-111">`IAuthorizationService`presenta due metodi, uno in cui si passa alla risorsa e il nome del criterio e l'altro in cui si passa la risorsa e un elenco di requisiti per la valutazione.</span><span class="sxs-lookup"><span data-stu-id="ed85d-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="ed85d-112">Per chiamare il servizio, caricare la risorsa all'interno dell'azione chiama quindi il `AuthorizeAsync` overload desiderate.</span><span class="sxs-lookup"><span data-stu-id="ed85d-112">To call the service, load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="ed85d-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ed85d-113">For example:</span></span>

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="ed85d-114">Scrittura di un gestore di risorse basato su</span><span class="sxs-lookup"><span data-stu-id="ed85d-114">Writing a resource based handler</span></span>

<span data-ttu-id="ed85d-115">Scrittura di un gestore per l'autorizzazione delle risorse, in base che non è molto diverso da [scrittura di un gestore di requisiti normale](policies.md#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="ed85d-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="ed85d-116">Si crea un requisito e implementa un gestore per il requisito, che specifica i requisiti come prima, nonché il tipo di risorsa.</span><span class="sxs-lookup"><span data-stu-id="ed85d-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="ed85d-117">Ad esempio, un gestore che potrebbe accettare una risorsa documento sarebbe come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ed85d-117">For example, a handler which might accept a Document resource would look as follows:</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="ed85d-118">Non dimenticare è inoltre necessario registrare il gestore nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="ed85d-118">Don't forget you also need to register your handler in the `ConfigureServices` method:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="ed85d-119">Requisiti operativi</span><span class="sxs-lookup"><span data-stu-id="ed85d-119">Operational Requirements</span></span>

<span data-ttu-id="ed85d-120">Se si apportano decisioni basate su operazioni quali la lettura, scrittura, aggiornamento ed eliminazione, è possibile utilizzare il `OperationAuthorizationRequirement` classe il `Microsoft.AspNetCore.Authorization.Infrastructure` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="ed85d-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="ed85d-121">Questa classe del requisito predefiniti consente di scrivere un singolo gestore che ha un nome di operazione con parametri, anziché creare classi singoli per ogni operazione.</span><span class="sxs-lookup"><span data-stu-id="ed85d-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="ed85d-122">Per utilizzarlo, fornire alcuni nomi di operazione:</span><span class="sxs-lookup"><span data-stu-id="ed85d-122">To use it, provide some operation names:</span></span>

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

<span data-ttu-id="ed85d-123">Il gestore possa quindi essere implementato come indicato di seguito, con un ipotetico `Document` classe della risorsa:</span><span class="sxs-lookup"><span data-stu-id="ed85d-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource:</span></span>

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="ed85d-124">È possibile visualizzare il funzionamento del gestore in `OperationAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="ed85d-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="ed85d-125">Il codice all'interno del gestore deve richiedere la proprietà Name del requisito fornito in considerazione quando si effettua la valutazione.</span><span class="sxs-lookup"><span data-stu-id="ed85d-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="ed85d-126">Per chiamare un gestore di risorse operative, è necessario specificare l'operazione quando si chiama `AuthorizeAsync` nell'ambito dell'azione.</span><span class="sxs-lookup"><span data-stu-id="ed85d-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="ed85d-127">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ed85d-127">For example:</span></span>

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

<span data-ttu-id="ed85d-128">Questo esempio viene verificato se l'utente è in grado di eseguire l'operazione di lettura per l'oggetto corrente `document` istanza.</span><span class="sxs-lookup"><span data-stu-id="ed85d-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="ed85d-129">La visualizzazione per il documento verrà restituita se l'autorizzazione ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="ed85d-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="ed85d-130">Se autorizzazione ha esito negativo restituendo `ChallengeResult` informa alcuna autenticazione middleware autorizzazione non è riuscita e il middleware può richiedere la risposta appropriata, ad esempio restituisce un codice di stato 401 o 403 o reindirizzando l'utente a una pagina di accesso per client del browser interattivo.</span><span class="sxs-lookup"><span data-stu-id="ed85d-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
