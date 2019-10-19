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
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorizzazione basata sulle risorse in ASP.NET Core

La strategia di autorizzazione dipende dalla risorsa a cui si accede. Si consideri un documento con una proprietà Author. Solo l'autore è autorizzato ad aggiornare il documento. Di conseguenza, il documento deve essere recuperato dall'archivio dati prima che possa essere eseguita la valutazione dell'autorizzazione.

La valutazione degli attributi viene eseguita prima data binding e prima dell'esecuzione del gestore di pagina o dell'azione che carica il documento. Per questi motivi, l'autorizzazione dichiarativa con un attributo `[Authorize]` non è sufficiente. È invece possibile richiamare un metodo di autorizzazione personalizzato &mdash;a stile definito *autorizzazione imperativa*.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([procedura per il download](xref:index#how-to-download-a-sample)).

[Creare un'app ASP.NET Core con i dati utente protetti dall'autorizzazione](xref:security/authorization/secure-data) contiene un'app di esempio che usa l'autorizzazione basata sulle risorse.

## <a name="use-imperative-authorization"></a>USA autorizzazione imperativa

L'autorizzazione viene implementata come servizio [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) e viene registrata nella raccolta di servizi all'interno della classe `Startup`. Il servizio viene reso disponibile tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) in gestori di pagine o azioni.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` dispone di due overload del metodo `AuthorizeAsync`: uno accetta la risorsa e il nome del criterio e l'altro accetta la risorsa e un elenco di requisiti da valutare.

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

Nell'esempio seguente la risorsa da proteggere viene caricata in un oggetto `Document` personalizzato. Viene richiamato un overload `AuthorizeAsync` per determinare se l'utente corrente è autorizzato a modificare il documento fornito. I criteri di autorizzazione "EditPolicy" personalizzati vengono presi in considerazione nella decisione. Per altre informazioni sulla creazione di criteri di autorizzazione, vedere [autorizzazione personalizzata basata su criteri](xref:security/authorization/policies) .

> [!NOTE]
> Negli esempi di codice seguenti si presuppone che l'autenticazione sia stata eseguita e che sia stata impostata la proprietà `User`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Scrivere un gestore basato su risorse

La scrittura di un gestore per l'autorizzazione basata sulle risorse non è molto diversa dalla [scrittura di un gestore di requisiti semplici](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Creare una classe di requisiti personalizzata e implementare una classe del gestore di requisiti. Per ulteriori informazioni sulla creazione di una classe requirement, vedere [requirements](xref:security/authorization/policies#requirements).

La classe handler specifica sia il requisito che il tipo di risorsa. Ad esempio, un gestore che utilizza un `SameAuthorRequirement` e una risorsa `Document` segue:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

Nell'esempio precedente, si supponga che `SameAuthorRequirement` sia un caso speciale di una classe `SpecificAuthorRequirement` più generica. La classe `SpecificAuthorRequirement` (non mostrata) contiene una proprietà `Name` che rappresenta il nome dell'autore. È possibile impostare la proprietà `Name` sull'utente corrente.

Registrare il requisito e il gestore in `Startup.ConfigureServices`:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisiti operativi

Se si stanno prendendo decisioni in base ai risultati delle operazioni CRUD (create, Read, Update, Delete), usare la classe helper [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) . Questa classe consente di scrivere un singolo gestore anziché una singola classe per ogni tipo di operazione. Per usarlo, specificare alcuni nomi di operazione:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Il gestore viene implementato come segue, usando un requisito `OperationAuthorizationRequirement` e una risorsa `Document`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

Il gestore precedente convalida l'operazione usando la risorsa, l'identità dell'utente e la proprietà `Name` del requisito.

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a>Verifica e Impedisci un gestore di risorse operativo

In questa sezione viene illustrato il modo in cui vengono elaborati i risultati dell'azione di richiesta e proibisce e il modo in cui le

Per chiamare un gestore di risorse operative, specificare l'operazione quando si richiama `AuthorizeAsync` nel gestore o nell'azione della pagina. Nell'esempio seguente viene determinato se l'utente autenticato è autorizzato a visualizzare il documento specificato.

> [!NOTE]
> Negli esempi di codice seguenti si presuppone che l'autenticazione sia stata eseguita e che sia stata impostata la proprietà `User`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Se l'autorizzazione ha esito positivo, viene restituita la pagina per la visualizzazione del documento. Se l'autorizzazione ha esito negativo, ma l'utente viene autenticato, restituendo `ForbidResult` informa qualsiasi middleware di autenticazione che l'autorizzazione non è riuscita. Quando è necessario eseguire l'autenticazione, viene restituito un `ChallengeResult`. Per i client del browser interattivo, potrebbe essere opportuno reindirizzare l'utente a una pagina di accesso.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Se l'autorizzazione ha esito positivo, viene restituita la vista per il documento. Se l'autorizzazione ha esito negativo, restituendo `ChallengeResult` informa il middleware di autenticazione che l'autorizzazione non è riuscita e il middleware può prendere la risposta appropriata. Una risposta appropriata potrebbe restituire un codice di stato 401 o 403. Per i client del browser interattivo, potrebbe significare il reindirizzamento dell'utente a una pagina di accesso.

::: moniker-end
