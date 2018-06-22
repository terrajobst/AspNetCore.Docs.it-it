---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e utilizzare i gestori di criteri di autorizzazione per applicare i requisiti di autorizzazione in un'applicazione ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 81ca6ad9ddba3de094762f5608bb6a5719bca7a1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277982"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorizzazione basata su criteri in ASP.NET Core

Nel sistema il [autorizzazione basata sui ruoli](xref:security/authorization/roles) e [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore del requisito e un criterio configurato in precedenza. Questi blocchi predefiniti supportano l'espressione di valutazioni di autorizzazione nel codice. Il risultato è una struttura di autorizzazioni complete, riutilizzabili e testabili.

Un criterio di autorizzazione è costituito da uno o più requisiti. È registrato come parte della configurazione del servizio di autorizzazione, il `Startup.ConfigureServices` metodo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

Nell'esempio precedente, viene creato un criterio di "AtLeast21". Ha un unico requisito&mdash;che di una durata minima, che viene fornita come parametro al requisito.

I criteri vengono applicati tramite il `[Authorize]` attributo con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Requisiti

Un requisito di autorizzazione è una raccolta di parametri dei dati che un criterio è possibile utilizzare per valutare l'entità utente corrente. In questo criterio "AtLeast21", il requisito è un singolo parametro&mdash;la data minima. Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuoto. Un requisito di età minima con parametri potrebbe essere implementato come indicato di seguito:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Non è un requisito a dati o proprietà.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestori di autorizzazione

Un gestore di autorizzazione è responsabile per la valutazione delle proprietà di un requisito. Il gestore autorizzazioni valuta i requisiti in un oggetto fornito [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) per determinare se è consentito l'accesso.

Può avere un requisito [più gestori](#security-authorization-policies-based-multiple-handlers). Può ereditare un gestore [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito deve essere gestito. In alternativa, è possibile implementare un gestore [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.

### <a name="use-a-handler-for-one-requirement"></a>Utilizzare un gestore per un requisito

<a name="security-authorization-handler-example"></a>

Di seguito è riportato un esempio di una relazione uno a uno in cui un gestore della durata minima prevede l'utilizzo di un unico requisito:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Il codice precedente determina se l'entità utente corrente dispone di una data di nascita di attestazioni che è stata eseguita da un'autorità emittente attendibile e nota. Autorizzazione non può verificarsi quando la richiesta è manca, nel qual caso viene restituita un'attività completata. Quando un'attestazione è presente, viene calcolato l'età dell'utente. Se l'utente soddisfi il periodo di memorizzazione minimo definito dai requisiti, autorizzazione viene considerata riuscita. Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito come unico parametro soddisfatto.

### <a name="use-a-handler-for-multiple-requirements"></a>Utilizzare un gestore per i requisiti di più

Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore autorizzazioni utilizza tre requisiti:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Consente di scorrere il codice precedente [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene i requisiti non contrassegnato come esito positivo. Se l'utente dispone dell'autorizzazione di lettura, che deve essere un proprietario o sponsor per accedere alla risorsa richiesta. Se l'utente dispone di modifica o Elimina autorizzazione, che deve essere un proprietario per accedere alla risorsa richiesta. Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito come unico parametro soddisfatto.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registrazione del gestore

Gestori eventi vengono registrati nella raccolta di servizi durante la configurazione. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Ogni gestore viene aggiunto alla raccolta di servizi richiamando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Cosa deve restituire un gestore?

Si noti che il `Handle` metodo il [esempio gestore](#security-authorization-handler-example) non restituisce alcun valore. Come viene riportato lo stato di esito positivo o un errore indicato?

* Indica l'esito positivo chiamando un gestore `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stata convalidata.

* Un gestore non è necessario gestire gli errori in genere, come altri gestori per lo stesso requisito può essere completata.

* Errore, anche se altri gestori di requisiti di esito positivo, chiamare `context.Fail`.

Se impostato su `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) proprietà (disponibile in ASP.NET Core 1.1 e versioni successive) provoca un corto circuito l'esecuzione di gestori quando `context.Fail` viene chiamato. `InvokeHandlersAfterFailure` Per impostazione predefinita `true`, nel qual caso tutti i gestori vengono chiamati. In questo modo i requisiti per produrre effetti collaterali, ad esempio la registrazione, che hanno luogo anche se `context.Fail` è stato chiamato in un altro gestore.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Perché desiderare più gestori per un requisito?

Nei casi in cui si desidera valutazione in un **o** base, implementare più gestori per un unico requisito. Microsoft ha ad esempio, le porte che viene aperta solo con schede di chiave. Se si lascia la smart card a casa, il centralinista Stampa etichetta della custodia temporanea e verrà aperta la porta. In questo scenario, è necessario un unico requisito *BuildingEntry*, ma più gestori, ciascuno di essi un unico requisito di analisi.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Assicurarsi che entrambi i gestori siano [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Valuta se ha esito positivo su gestore, quando un criterio di `BuildingEntryRequirement`, ha esito positivo della valutazione dei criteri.

## <a name="using-a-func-to-fulfill-a-policy"></a>Utilizzo di func per soddisfare i criteri di

Potrebbero verificarsi situazioni in cui che soddisfano un criterio è semplice da express nel codice. È possibile fornire un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.

Ad esempio, il precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Accesso al contesto di richiesta MVC nei gestori

Il `HandleRequirementAsync` metodo è implementato in un gestore di autorizzazione presenta due parametri: un `AuthorizationHandlerContext` e `TRequirement` si sta gestendo. Framework di MVC o Jabbr hanno la possibilità di aggiungere qualsiasi oggetto di `Resource` proprietà il `AuthorizationHandlerContext` per passare informazioni aggiuntive.

Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nel `Resource` proprietà. Questa proprietà fornisce l'accesso a `HttpContext`, `RouteData`e tutto ciò che altrimenti forniti da MVC e pagine Razor.

L'utilizzo del `Resource` è di proprietà specifici del framework. Utilizzando le informazioni nel `Resource` proprietà limita i criteri di autorizzazione per un framework specifico. È necessario eseguire il cast di `Resource` proprietà utilizzando il `as` (parola chiave) e quindi confermare il cast ha esito positivo per verificare che il codice non di arresto anomalo con un `InvalidCastException` quando viene eseguito da altri Framework:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
