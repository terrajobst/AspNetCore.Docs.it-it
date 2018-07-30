---
title: Autorizzazione basata sulle risorse in ASP.NET Core
author: scottaddie
description: Informazioni su come implementare l'autorizzazione basata sulle risorse in un'app ASP.NET Core quando non sono sufficienti un attributo Authorize.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a110a69c58d5e20a15198378510486daec3d452
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342289"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorizzazione basata sulle risorse in ASP.NET Core

Strategia di autorizzazione varia a seconda della risorsa a cui si accede. Si consideri un documento che ha una proprietà dell'autore. Solo l'autore può aggiornare il documento. Di conseguenza, il documento deve essere recuperato dall'archivio dati prima di poter eseguire la valutazione di autorizzazione.

Attributo valutazioni prima del data binding e prima dell'esecuzione del gestore di pagina o l'azione che carica il documento. Per questi motivi, autorizzazione dichiarative con un `[Authorize]` attributo non sono sufficienti. In alternativa, è possibile richiamare un metodo di autorizzazione personalizzato&mdash;uno stile noto come autorizzazione imperativo.

Usare la [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) per esplorare le funzionalità descritte in questo argomento.

[Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che utilizza l'autorizzazione basata sulle risorse.

## <a name="use-imperative-authorization"></a>Usare l'autorizzazione imperativo

Autorizzazione viene implementato come un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) del servizio e viene registrato nell'insieme del servizio all'interno di `Startup` classe. Il servizio viene reso disponibile tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) a gestori di pagina o azioni.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` ha due `AuthorizeAsync` overload dei metodi: uno accetta la risorsa e il nome del criterio e l'altro accetta un elenco di requisiti per la valutazione e la risorsa.

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

Nell'esempio seguente, la risorsa deve essere protetto viene caricata in una classe personalizzata `Document` oggetto. Un `AuthorizeAsync` overload viene richiamato per determinare se l'utente corrente è autorizzato a modificare il documento specificato. Un criterio di autorizzazione "EditPolicy" personalizzato è inserito nella decisione. Visualizzare [autorizzazione basata sui criteri personalizzati](xref:security/authorization/policies) per altre informazioni sulla creazione di criteri di autorizzazione.

> [!NOTE]
> Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Scrivere un gestore basata sulle risorse

Scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diverso da quello [scrivendo un gestore di requisiti semplici](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Creare una classe del requisito personalizzato e implementare una classe del requisito del gestore. La classe del gestore consente di specificare sia il requisito e il tipo di risorsa. Ad esempio, un gestore che usano un `SameAuthorRequirement` requisito e un `Document` risorse è simile al seguente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Registrare un gestore in e il requisito di `Startup.ConfigureServices` metodo:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisiti operativi

Se si stanno apportando decisioni basate sui risultati di operazioni CRUD (Create, Read, Update, Delete), usare il [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe helper. Questa classe consente di scrivere un singolo gestore invece di una classe distinta per ogni tipo di operazione. Per usarlo, fornire alcuni nomi di operazione:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Il gestore di è implementato come segue, usando un `OperationAuthorizationRequirement` requisito e un `Document` risorsa:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Il gestore precedente convalida l'operazione usando la risorsa, l'identità dell'utente e il requisito `Name` proprietà.

Per chiamare un gestore di risorse operativa, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore di pagina o nell'azione. Nell'esempio seguente determina se l'utente autenticato ha la possibilità di visualizzare il documento specificato.

> [!NOTE]
> Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento. Se si verifica un errore di autorizzazione, ma l'utente viene autenticato, restituendo `ForbidResult` indica a qualsiasi middleware di autenticazione che autorizzazione non riuscita. Oggetto `ChallengeResult` viene restituito quando l'autenticazione deve essere eseguita. Per i client browser interattiva, potrebbe essere appropriata reindirizzare l'utente a una pagina di accesso.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Se l'autorizzazione ha esito positivo, viene restituita la visualizzazione del documento. Se l'autorizzazione ha esito negativo, restituendo `ChallengeResult` informa qualsiasi middleware di autenticazione che autorizzazione non riuscita e che il middleware può richiedere la risposta appropriata. Una risposta appropriata potrebbe restituire un codice di stato 401 o 403. Per i client browser interattiva, è possibile reindirizzare l'utente a una pagina di accesso.

---
