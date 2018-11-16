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
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorizzazione basata sulle risorse in ASP.NET Core

Strategia di autorizzazione varia a seconda della risorsa a cui si accede. Si consideri un documento contenente una proprietà dell'autore. Solo l'autore può aggiornare il documento. Di conseguenza, il documento deve essere recuperato dall'archivio dati prima di poter eseguire la valutazione di autorizzazione.

Attributo valutazioni prima del data binding e prima dell'esecuzione del gestore di pagina o azione che carica il documento. Per questi motivi, autorizzazione dichiarative con un `[Authorize]` attributo non sono sufficienti. In alternativa, è possibile richiamare un metodo di autorizzazione personalizzato&mdash;uno stile detto *autorizzazione imperativo*.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).

[Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che utilizza l'autorizzazione basata sulle risorse.

## <a name="use-imperative-authorization"></a>Usare l'autorizzazione imperativo

Autorizzazione viene implementato come un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) del servizio e viene registrato nell'insieme del servizio all'interno di `Startup` classe. Il servizio viene reso disponibile tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) a gestori di pagina o azioni.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` ha due `AuthorizeAsync` overload dei metodi: uno accetta la risorsa e il nome del criterio e l'altro accetta un elenco di requisiti per la valutazione e la risorsa.

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

Nell'esempio seguente, la risorsa deve essere protetto viene caricata in una classe personalizzata `Document` oggetto. Un `AuthorizeAsync` overload viene richiamato per determinare se l'utente corrente è autorizzato a modificare il documento specificato. Un criterio di autorizzazione "EditPolicy" personalizzato è inserito nella decisione. Visualizzare [autorizzazione basata sui criteri personalizzati](xref:security/authorization/policies) per altre informazioni sulla creazione di criteri di autorizzazione.

> [!NOTE]
> Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Scrivere un gestore basata sulle risorse

Scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diverso da quello [scrivendo un gestore di requisiti semplici](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Creare una classe del requisito personalizzato e implementare una classe del requisito del gestore. Per altre informazioni sulla creazione di una classe del requisito, vedere [requisiti](xref:security/authorization/policies#requirements).

La classe del gestore consente di specificare sia il requisito e il tipo di risorsa. Ad esempio, un gestore che usano un `SameAuthorRequirement` e un `Document` risorse indicato di seguito:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

Nell'esempio precedente, si supponga `SameAuthorRequirement` è un caso speciale di un oggetto generico più `SpecificAuthorRequirement` classe. Il `SpecificAuthorRequirement` classe (non mostrato) contiene un `Name` proprietà che rappresenta il nome dell'autore. Il `Name` proprietà può essere impostata per l'utente corrente.

Registrare il requisito e il gestore in `Startup.ConfigureServices`:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisiti operativi

Se si stanno apportando decisioni basate sui risultati di operazioni CRUD (Create, Read, Update, Delete), usare il [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe helper. Questa classe consente di scrivere un singolo gestore invece di una classe distinta per ogni tipo di operazione. Per usarlo, fornire alcuni nomi di operazione:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Il gestore di è implementato come segue, usando un `OperationAuthorizationRequirement` requisito e un `Document` risorsa:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

Il gestore precedente convalida l'operazione usando la risorsa, l'identità dell'utente e il requisito `Name` proprietà.

Per chiamare un gestore di risorse operativa, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore di pagina o nell'azione. Nell'esempio seguente determina se l'utente autenticato ha la possibilità di visualizzare il documento specificato.

> [!NOTE]
> Il codice riportato di seguito esempi presumono è stata eseguita l'autenticazione e impostare il `User` proprietà.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento. Se si verifica un errore di autorizzazione, ma l'utente viene autenticato, restituendo `ForbidResult` indica a qualsiasi middleware di autenticazione che autorizzazione non riuscita. Oggetto `ChallengeResult` viene restituito quando l'autenticazione deve essere eseguita. Per i client browser interattiva, potrebbe essere appropriata reindirizzare l'utente a una pagina di accesso.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Se l'autorizzazione ha esito positivo, viene restituita la visualizzazione del documento. Se l'autorizzazione ha esito negativo, restituendo `ChallengeResult` informa qualsiasi middleware di autenticazione che autorizzazione non riuscita e che il middleware può richiedere la risposta appropriata. Una risposta appropriata potrebbe restituire un codice di stato 401 o 403. Per i client browser interattiva, è possibile reindirizzare l'utente a una pagina di accesso.

::: moniker-end
