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
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a>Autorizzazione basata sulle risorse

<a name=security-authorization-resource-based></a>

Autorizzazione spesso dipende dalla risorsa a cui accedere. Ad esempio un documento può disporre di una proprietà dell'autore. Solo l'autore del documento sarebbe possibile aggiornarlo, pertanto la risorsa deve essere caricata dal repository di documenti prima di effettuare una valutazione di autorizzazione. Questa operazione non può essere eseguita con l'attributo Authorize come attributo valutazione ha luogo prima del data binding e prima dell'esecuzione di codice per caricare una risorsa all'interno di un'azione. Invece di autorizzazione dichiarativa, il metodo di attributo, è necessario utilizzare l'autorizzazione imperativa, in cui uno sviluppatore chiama una funzione di authorize interno del codice.

## <a name="authorizing-within-your-code"></a>Autorizzazione all'interno del codice

Autorizzazione viene implementata come un servizio, `IAuthorizationService`, registrato nell'insieme di servizio e disponibile tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) per i controller per accedere.

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

`IAuthorizationService`presenta due metodi, uno in cui si passa alla risorsa e il nome del criterio e l'altro in cui si passa la risorsa e un elenco di requisiti per la valutazione.

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

Per chiamare il carico del servizio della risorsa all'interno dell'azione chiama quindi il `AuthorizeAsync` overload desiderate. Esempio:

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

## <a name="writing-a-resource-based-handler"></a>Scrittura di un gestore di risorse basato su

Scrittura di un gestore per l'autorizzazione delle risorse, in base che non è molto diverso da [scrittura di un gestore di requisiti normale](policies.md#security-authorization-policies-based-authorization-handler). Si crea un requisito e implementa un gestore per il requisito, che specifica i requisiti come prima, nonché il tipo di risorsa. Ad esempio, un gestore che potrebbe accettare una risorsa documento potrebbe essere come segue:

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

Non dimenticare è inoltre necessario registrare il gestore nel `ConfigureServices` ; (metodo)

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>Requisiti operativi

Se si apportano decisioni basate su operazioni quali la lettura, scrittura, aggiornamento ed eliminazione, è possibile utilizzare il `OperationAuthorizationRequirement` classe il `Microsoft.AspNetCore.Authorization.Infrastructure` dello spazio dei nomi. Questa classe del requisito predefiniti consente di scrivere un singolo gestore che ha un nome di operazione con parametri, anziché creare classi singoli per ogni operazione. Per utilizzare fornisce alcuni nomi di operazione:

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

Il gestore possa quindi essere implementato come indicato di seguito, con un ipotetico `Document` classe come risorsa.

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

È possibile visualizzare il funzionamento del gestore in `OperationAuthorizationRequirement`. Il codice all'interno del gestore deve richiedere la proprietà Name del requisito fornito in considerazione quando si effettua la valutazione.

Per chiamare un gestore di risorse operative, è necessario specificare l'operazione quando si chiama `AuthorizeAsync` nell'ambito dell'azione. Esempio:

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

Questo esempio viene verificato se l'utente è in grado di eseguire l'operazione di lettura per l'oggetto corrente `document` istanza. La visualizzazione per il documento verrà restituita se l'autorizzazione ha esito positivo. Se autorizzazione ha esito negativo restituendo `ChallengeResult` informa alcuna autenticazione middleware autorizzazione non è riuscita e il middleware può richiedere la risposta appropriata, ad esempio restituisce un codice di stato 401 o 403 o reindirizzando l'utente a una pagina di accesso per client del browser interattivo.
