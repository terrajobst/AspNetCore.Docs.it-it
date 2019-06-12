---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e utilizzare i gestori di criteri di autorizzazione per l'applicazione dei requisiti di autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837347"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorizzazione basata su criteri in ASP.NET Core

Sotto le quinte [autorizzazione basata sui ruoli](xref:security/authorization/roles) e [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore di requisito e un criterio preconfigurato. Questi blocchi predefiniti supportano l'espressione di valutazioni di autorizzazione nel codice. Il risultato è una struttura di autorizzazione più avanzate, riutilizzabili e testabili.

Un criterio di autorizzazione è costituito da uno o più requisiti. In cui è registrata come parte della configurazione del servizio di autorizzazione, il `Startup.ConfigureServices` metodo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

Nell'esempio precedente, viene creato un criterio di "AtLeast21". Ha un unico requisito&mdash;che di una durata minima, che viene fornita come parametro al requisito.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Il servizio primario che determina se l'autorizzazione ha esito positivo è <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Il codice precedente evidenzia due metodi per la [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> è un servizio marcatore senza metodi e il meccanismo di rilevamento se l'autorizzazione ha esito positivo.

Ogni <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> è responsabile della verifica se vengono soddisfatti i requisiti:
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

Il <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> classe viene usato il gestore per contrassegnare se sono stati soddisfatti i requisiti:

```csharp
 context.Succeed(requirement)
```

Il codice seguente illustra il semplificata e annotato con commenti, l'implementazione del servizio di autorizzazione predefinita:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

Il codice seguente viene illustrato un tipico `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

Uso <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something"]` per l'autorizzazione.

## <a name="applying-policies-to-mvc-controllers"></a>Applicare criteri ai controller MVC

Se si usano le pagine Razor, vedere [applicazione di criteri a Razor Pages](#applying-policies-to-razor-pages) in questo documento.

I criteri vengono applicati ai controller usando il `[Authorize]` attributo con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Applicare criteri a Razor Pages

I criteri vengono applicati a Razor Pages usando il `[Authorize]` attributo con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Criteri possono essere applicati anche alle pagine Razor con un [convenzione autorizzazione](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Requisiti

Un requisito di autorizzazione è una raccolta di parametri di dati che i criteri possono utilizzare per valutare l'entità utente corrente. Nei nostri criteri "AtLeast21", il requisito è un singolo parametro&mdash;la validità minima. Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuoto. Un requisito di validità minima con parametri potrebbe essere implementato come segue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Se un criterio di autorizzazione contiene più i requisiti di autorizzazione, è necessario passare tutti i requisiti in ordine per la valutazione dei criteri abbia esito positivo. In altre parole, più i requisiti di autorizzazione aggiunti a un singolo criterio di autorizzazione vengono considerati in un **AND** base.

> [!NOTE]
> Un requisito non deve necessariamente avere dei dati o le proprietà.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestori di autorizzazione

Un gestore di autorizzazione è responsabile per la valutazione delle proprietà del requisito. Il gestore autorizzazioni valuta i requisiti per un oggetto fornito [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) per determinare se l'accesso è consentito.

Può avere un requisito [più gestori](#security-authorization-policies-based-multiple-handlers). Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito per essere gestiti. In alternativa, è possibile implementare un gestore [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.

### <a name="use-a-handler-for-one-requirement"></a>Usare un gestore per uno dei requisiti

<a name="security-authorization-handler-example"></a>

Di seguito è riportato un esempio di una relazione uno a uno in cui un gestore della durata minima utilizza un unico requisito:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Il codice precedente determina se l'entità utente corrente ha una data di nascita di attestazione che sia stato emesso da un'autorità emittente conosciuta e attendibile. Autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata. Quando un'attestazione è presente, viene calcolato l'età dell'utente. Se l'utente soddisfa il numero minimo giorni definito dai requisiti, autorizzazione venga considerata riuscita. Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.

### <a name="use-a-handler-for-multiple-requirements"></a>Usare un gestore per i requisiti di più

Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazione possa gestire tre diversi tipi di requisiti:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene i requisiti non contrassegnata come positiva. Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta. Nel caso di un' `EditPermission` o `DeletePermission` requisito, che potrà deve essere un proprietario per accedere alla risorsa richiesta.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registrazione del gestore

I gestori registrati dell'insieme di servizi durante la configurazione. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Il codice precedente registra `MinimumAgeHandler` come un singleton richiamando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Gestori possono essere registrati usando uno dei integrato [durate del servizio](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Che cosa deve restituire un gestore?

Si noti che il `Handle` metodo nella [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore. Come è stato positivo o negativo indicato?

* Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che non è stata convalidata.

* Un gestore non è necessario gestire gli errori a livello generale, come altri gestori per gli stessi requisiti potrebbero avere esito positivo.

* Per garantire il caso di errore, anche se altri gestori di requisiti abbia esito positivo, chiamare `context.Fail`.

Se chiama un gestore `context.Succeed` o `context.Fail`, tutti gli altri gestori sono ancora chiamati. In questo modo i requisiti per produrre effetti collaterali, ad esempio la registrazione, che viene eseguita anche se un altro gestore ha convalidato correttamente o non riuscita di un requisito. Se impostato su `false`, il [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) proprietà (disponibile in ASP.NET Core 1.1 e versioni successive) consente di bloccare l'esecuzione dei gestori quando `context.Fail` viene chiamato. `InvokeHandlersAfterFailure` Per impostazione predefinita `true`, nel qual caso tutti i gestori vengono chiamati.

> [!NOTE]
> Gestori di autorizzazione vengono chiamati anche se l'autenticazione non riesce.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Il motivo per cui è consigliabile più gestori per un requisito?

Nei casi in cui si desidera valutazione in un **o** singoli, implementare più gestori per un unico requisito. Ad esempio, Microsoft ha le porte che essere aperto solo con le schede principali. Se si lascia la smart card a casa, l'addetto ai ricoveri viene stampato un adesivo temporaneo e apre le porte per l'utente. In questo scenario sarebbe necessario un unico requisito *BuildingEntry*, ma più gestori, ciascuno di essi esaminando un unico requisito.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Assicurarsi che entrambi i gestori [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Se ha esito positivo su gestore quando un criterio valuta le `BuildingEntryRequirement`, ha esito positivo della valutazione dei criteri.

## <a name="using-a-func-to-fulfill-a-policy"></a>Usando una funzione per soddisfare un criterio

Si possono verificare situazioni in cui che soddisfano un criterio è semplice da esprimere nel codice. È possibile fornire un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.

Ad esempio, il precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Accesso al contesto di richiesta MVC nei gestori

Il `HandleRequirementAsync` metodo si implementa in un gestore di autorizzazione ha due parametri: un `AuthorizationHandlerContext` e il `TRequirement` si sta gestendo. Framework come MVC o Jabbr sono liberi di aggiunta di un oggetto per il `Resource` proprietà il `AuthorizationHandlerContext` per passare informazioni aggiuntive.

Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nel `Resource` proprietà. Questa proprietà consente di accedervi `HttpContext`, `RouteData`e tutto quello che altrimenti fornito da MVC e Razor Pages.

L'uso del `Resource` proprietà è framework specifico. Utilizzando le informazioni nel `Resource` proprietà limita i criteri di autorizzazione al framework specifico. È necessario eseguire il cast di `Resource` proprietà usando la `is` (parola chiave) e quindi confermare il cast ha avuto esito positivo per assicurarsi che il codice non di arresto anomalo con un `InvalidCastException` quando eseguono in altri Framework:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
