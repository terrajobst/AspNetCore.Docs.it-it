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
ms.openlocfilehash: 3be2d9b9aef18763fbdba78e044dd6b68ddcef0c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094282"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorizzazione basata sulle risorse in ASP.NET Core

Strategia di autorizzazione dipende dalla risorsa a cui accedere. Si consideri un documento che contiene una proprietà dell'autore. È consentito solo l'autore di aggiornare il documento. Di conseguenza, il documento deve essere recuperato dall'archivio dati prima di poter eseguire la valutazione di autorizzazione.

Attributo valutazione si verifica prima dell'associazione a dati e prima dell'esecuzione del gestore di pagina o azione che carica il documento. Per questi motivi, l'autorizzazione dichiarativa con un `[Authorize]` attributo non sono sufficienti. In alternativa, è possibile richiamare un metodo di autorizzazione personalizzato&mdash;uno stile noto come autorizzazione imperativa.

Utilizzare il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) per esplorare le funzionalità descritte in questo argomento.

[Crea un'applicazione ASP.NET Core con dati dell'utente protetti dall'autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che utilizza l'autorizzazione basata sulle risorse.

## <a name="use-imperative-authorization"></a>Utilizzare l'autorizzazione imperativo

Autorizzazione viene implementata come un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) del servizio e viene registrato nell'insieme di servizio all'interno di `Startup` classe. Il servizio viene reso disponibile tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) a gestori di pagina o azioni.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` dispone di due `AuthorizeAsync` overload del metodo: uno che accetta la risorsa e il nome del criterio e l'altro che accetta la risorsa e un elenco dei requisiti da valutare.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

Nell'esempio seguente, la risorsa da proteggere viene caricata in un oggetto personalizzato `Document` oggetto. Un `AuthorizeAsync` overload viene richiamato per determinare se l'utente corrente è autorizzato a modificare il documento specificato. Un criterio di autorizzazione personalizzato "EditPolicy" è inserito nella decisione. Vedere [autorizzazione basata su criteri personalizzati](xref:security/authorization/policies) per ulteriori informazioni sulla creazione di criteri di autorizzazione.

> [!NOTE]
> Il codice seguente si supponga di esempi è stata eseguita l'autenticazione e un set di `User` proprietà.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Scrivere un gestore di risorse

Scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diverso rispetto a [scrittura di un gestore di requisiti normale](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Creare una classe del requisito personalizzata e implementare una classe del requisito del gestore. La classe del gestore specifica sia il requisito e il tipo di risorsa. Ad esempio, un gestore che utilizzano un `SameAuthorRequirement` requisito e un `Document` risorse è simile al seguente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Registrare un gestore in e il requisito di `Startup.ConfigureServices` metodo:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisiti operativi

Se si apportano le decisioni in base ai risultati di operazioni CRUD (Create, Read, Update, Delete), usare il [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe helper. Questa classe consente di scrivere un singolo gestore anziché una singola classe per ogni tipo di operazione. Per utilizzarlo, fornire alcuni nomi di operazione:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Il gestore viene implementato come indicato di seguito, utilizzando un `OperationAuthorizationRequirement` requisito e un `Document` risorse:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Il gestore precedente convalida l'operazione utilizzando la risorsa, l'identità dell'utente e il requisito `Name` proprietà.

Per chiamare un gestore di risorse operative, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore di pagina o azione. Nell'esempio seguente determina se l'utente autenticato può visualizzare il documento specificato.

> [!NOTE]
> Il codice seguente si supponga di esempi è stata eseguita l'autenticazione e un set di `User` proprietà.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento. Se si verifica un errore di autorizzazione, ma l'utente viene autenticato, restituendo `ForbidResult` informa qualsiasi middleware di autenticazione che l'autorizzazione non riuscita. Oggetto `ChallengeResult` viene restituito quando l'autenticazione deve essere eseguita. Per i client del browser interattivo, potrebbe essere opportuno reindirizzare l'utente a una pagina di accesso.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Se l'autorizzazione ha esito positivo, viene restituita la visualizzazione del documento. Se l'autorizzazione ha esito negativo, restituisce `ChallengeResult` indica qualsiasi middleware di autenticazione di autorizzazione non riuscita, il middleware può richiedere la risposta appropriata. Impossibile restituire un codice di stato 401 o 403 una risposta appropriata. Per i client del browser interattiva, è possibile che il reindirizzamento dell'utente a una pagina di accesso.

---
