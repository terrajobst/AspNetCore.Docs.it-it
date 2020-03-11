---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e usare i gestori dei criteri di autorizzazione per applicare i requisiti di autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661812"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorizzazione basata su criteri in ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Dietro le quinte, l'autorizzazione [basata sui ruoli](xref:security/authorization/roles) e l' [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore requisiti e un criterio preconfigurato. Questi blocchi predefiniti supportano l'espressione delle valutazioni di autorizzazione nel codice. Il risultato è una struttura di autorizzazione verificabile, riutilizzabile e più ricca.

Un criterio di autorizzazione è costituito da uno o più requisiti. Viene registrato come parte della configurazione del servizio di autorizzazione, nel metodo `Startup.ConfigureServices`:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

Nell'esempio precedente viene creato un criterio "AtLeast21". Dispone di un singolo requisito&mdash;quello di una validità minima, che viene fornito come parametro al requisito.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Il servizio primario che determina se l'autorizzazione ha esito positivo è <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Il codice precedente evidenzia i due metodi di [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> è un servizio marcatore senza metodi e il meccanismo per verificare se l'autorizzazione ha avuto esito positivo.

Ogni <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> è responsabile per verificare se sono soddisfatti i requisiti:
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

La classe <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> è l'elemento usato dal gestore per contrassegnare se sono stati soddisfatti i requisiti:

```csharp
 context.Succeed(requirement)
```

Nel codice seguente viene illustrata l'implementazione predefinita del servizio di autorizzazione semplificata (e annotata con commenti):

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

Il codice seguente illustra un tipico `ConfigureServices`:

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


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

Utilizzare <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something")]` per l'autorizzazione.

## <a name="applying-policies-to-mvc-controllers"></a>Applicazione di criteri ai controller MVC

Se si usa Razor Pages, vedere [applicazione di criteri ai Razor Pages](#applying-policies-to-razor-pages) in questo documento.

I criteri vengono applicati ai controller usando l'attributo `[Authorize]` con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Applicazione di criteri a Razor Pages

I criteri vengono applicati a Razor Pages usando l'attributo `[Authorize]` con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

I criteri possono essere applicati anche a Razor Pages usando una [convenzione di autorizzazione](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Requisiti

Un requisito di autorizzazione è una raccolta di parametri dati che un criterio può utilizzare per valutare l'entità utente corrente. Nel criterio "AtLeast21", il requisito è un singolo parametro&mdash;età minima. Un requisito implementa [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuota. Un requisito di validità minima con parametri può essere implementato come segue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Se un criterio di autorizzazione contiene più requisiti di autorizzazione, tutti i requisiti devono essere superati affinché la valutazione dei criteri abbia esito positivo. In altre parole, i requisiti di autorizzazione multipli aggiunti a un singolo criterio di autorizzazione vengono gestiti in base a **e** .

> [!NOTE]
> Un requisito non deve avere dati o proprietà.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestori autorizzazioni

Un gestore di autorizzazione è responsabile della valutazione delle proprietà di un requisito. Il gestore di autorizzazione valuta i requisiti rispetto a un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) specificato per determinare se l'accesso è consentito.

Un requisito può avere [più gestori](#security-authorization-policies-based-multiple-handlers). Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito da gestire. In alternativa, un gestore può implementare [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.

### <a name="use-a-handler-for-one-requirement"></a>Usare un gestore per un requisito

<a name="security-authorization-handler-example"></a>

Di seguito è riportato un esempio di una relazione uno-a-uno in cui un gestore età minima utilizza un singolo requisito:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Il codice precedente determina se l'entità utente corrente ha una data di attestazione di nascita che è stata rilasciata da un'autorità emittente nota ed attendibile. L'autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata. Quando è presente un'attestazione, viene calcolata l'età dell'utente. Se l'utente soddisfa la validità minima definita dal requisito, l'autorizzazione viene ritenuta corretta. Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.

### <a name="use-a-handler-for-multiple-requirements"></a>Usare un gestore per più requisiti

Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazioni è in grado di gestire tre diversi tipi di requisiti:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene requisiti non contrassegnati come riusciti. Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta. Nel caso di un requisito `EditPermission` o `DeletePermission`, deve essere un proprietario per accedere alla risorsa richiesta.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registrazione del gestore

I gestori vengono registrati nella raccolta di servizi durante la configurazione. Ad esempio:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

Il codice precedente registra `MinimumAgeHandler` come singleton richiamando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. I gestori possono essere registrati usando una qualsiasi delle [durate dei servizi](xref:fundamentals/dependency-injection#service-lifetimes)predefinite.

## <a name="what-should-a-handler-return"></a>Che cosa deve restituire un gestore?

Si noti che il metodo `Handle` nell' [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore. Come viene indicato lo stato di esito positivo o negativo?

* Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stato convalidato correttamente.

* Un gestore non deve gestire gli errori in modo generale, perché gli altri gestori per lo stesso requisito possono avere esito positivo.

* Per garantire un errore, anche se gli altri gestori di requisiti hanno esito positivo, chiamare `context.Fail`.

Se un gestore chiama `context.Succeed` o `context.Fail`, tutti gli altri gestori vengono ancora chiamati. Questo consente ai requisiti di produrre effetti collaterali, ad esempio la registrazione, che si verifica anche se un altro gestore ha correttamente convalidato o non ha superato un requisito. Se impostato su `false`, la proprietà [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (disponibile in ASP.NET Core 1,1 e versioni successive) esegue il cortocircuito dell'esecuzione dei gestori quando viene chiamato `context.Fail`. `InvokeHandlersAfterFailure` viene impostato il valore predefinito `true`, nel qual caso vengono chiamati tutti i gestori.

> [!NOTE]
> I gestori di autorizzazione vengono chiamati anche se l'autenticazione ha esito negativo.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Perché si vogliono usare più gestori per un requisito?

Nei casi in cui si desidera che la valutazione sia basata su **o** , implementare più gestori per un singolo requisito. Ad esempio, Microsoft dispone di porte che si aprono solo con le schede chiave. Se si lascia la scheda chiave a casa, il receptionist stampa un adesivo temporaneo e apre lo sportello. In questo scenario si avrà un singolo requisito, *BuildingEntry*, ma più gestori, ognuno di essi esaminando un singolo requisito.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Assicurarsi che entrambi i gestori siano [registrati](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Se uno dei due gestori ha esito positivo quando un criterio valuta il `BuildingEntryRequirement`, la valutazione dei criteri ha esito positivo.

## <a name="using-a-func-to-fulfill-a-policy"></a>Uso di un Func per soddisfare un criterio

In alcuni casi è possibile che l'evasione di un criterio sia semplice da esprimere nel codice. È possibile specificare un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il generatore di criteri di `RequireAssertion`.

Ad esempio, il `BadgeEntryHandler` precedente potrebbe essere riscritto come segue:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Accesso al contesto della richiesta MVC nei gestori

Il metodo `HandleRequirementAsync` implementato in un gestore autorizzazioni presenta due parametri: un `AuthorizationHandlerContext` e il `TRequirement` che si sta gestendo. I Framework come MVC o Jabbr sono liberi di aggiungere qualsiasi oggetto alla proprietà `Resource` nel `AuthorizationHandlerContext` per passare informazioni aggiuntive.

Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nella proprietà `Resource`. Questa proprietà consente di accedere a `HttpContext`, `RouteData`e tutto il resto fornito da MVC e Razor Pages.

L'uso della proprietà `Resource` è specifico del Framework. L'uso delle informazioni nella proprietà `Resource` limita i criteri di autorizzazione a specifici Framework. È necessario eseguire il cast della proprietà `Resource` usando la parola chiave `is`, quindi verificare che il cast abbia avuto esito positivo per assicurarsi che il codice non si arresti in modo anomalo con un `InvalidCastException` quando viene eseguito in altri Framework:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

Dietro le quinte, l'autorizzazione [basata sui ruoli](xref:security/authorization/roles) e l' [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore requisiti e un criterio preconfigurato. Questi blocchi predefiniti supportano l'espressione delle valutazioni di autorizzazione nel codice. Il risultato è una struttura di autorizzazione verificabile, riutilizzabile e più ricca.

Un criterio di autorizzazione è costituito da uno o più requisiti. Viene registrato come parte della configurazione del servizio di autorizzazione, nel metodo `Startup.ConfigureServices`:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

Nell'esempio precedente viene creato un criterio "AtLeast21". Dispone di un singolo requisito&mdash;quello di una validità minima, che viene fornito come parametro al requisito.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Il servizio primario che determina se l'autorizzazione ha esito positivo è <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Il codice precedente evidenzia i due metodi di [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> è un servizio marcatore senza metodi e il meccanismo per verificare se l'autorizzazione ha avuto esito positivo.

Ogni <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> è responsabile per verificare se sono soddisfatti i requisiti:
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

La classe <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> è l'elemento usato dal gestore per contrassegnare se sono stati soddisfatti i requisiti:

```csharp
 context.Succeed(requirement)
```

Nel codice seguente viene illustrata l'implementazione predefinita del servizio di autorizzazione semplificata (e annotata con commenti):

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

Il codice seguente illustra un tipico `ConfigureServices`:

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

Utilizzare <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something")]` per l'autorizzazione.

## <a name="applying-policies-to-mvc-controllers"></a>Applicazione di criteri ai controller MVC

Se si usa Razor Pages, vedere [applicazione di criteri ai Razor Pages](#applying-policies-to-razor-pages) in questo documento.

I criteri vengono applicati ai controller usando l'attributo `[Authorize]` con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Applicazione di criteri a Razor Pages

I criteri vengono applicati a Razor Pages usando l'attributo `[Authorize]` con il nome del criterio. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

I criteri possono essere applicati anche a Razor Pages usando una [convenzione di autorizzazione](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Requisiti

Un requisito di autorizzazione è una raccolta di parametri dati che un criterio può utilizzare per valutare l'entità utente corrente. Nel criterio "AtLeast21", il requisito è un singolo parametro&mdash;età minima. Un requisito implementa [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuota. Un requisito di validità minima con parametri può essere implementato come segue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Se un criterio di autorizzazione contiene più requisiti di autorizzazione, tutti i requisiti devono essere superati affinché la valutazione dei criteri abbia esito positivo. In altre parole, i requisiti di autorizzazione multipli aggiunti a un singolo criterio di autorizzazione vengono gestiti in base a **e** .

> [!NOTE]
> Un requisito non deve avere dati o proprietà.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestori autorizzazioni

Un gestore di autorizzazione è responsabile della valutazione delle proprietà di un requisito. Il gestore di autorizzazione valuta i requisiti rispetto a un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) specificato per determinare se l'accesso è consentito.

Un requisito può avere [più gestori](#security-authorization-policies-based-multiple-handlers). Un gestore può ereditare [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito da gestire. In alternativa, un gestore può implementare [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.

### <a name="use-a-handler-for-one-requirement"></a>Usare un gestore per un requisito

<a name="security-authorization-handler-example"></a>

Di seguito è riportato un esempio di una relazione uno-a-uno in cui un gestore età minima utilizza un singolo requisito:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Il codice precedente determina se l'entità utente corrente ha una data di attestazione di nascita che è stata rilasciata da un'autorità emittente nota ed attendibile. L'autorizzazione non può verificarsi quando l'attestazione non è presente, nel qual caso viene restituita un'attività completata. Quando è presente un'attestazione, viene calcolata l'età dell'utente. Se l'utente soddisfa la validità minima definita dal requisito, l'autorizzazione viene ritenuta corretta. Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito soddisfatto come unico parametro.

### <a name="use-a-handler-for-multiple-requirements"></a>Usare un gestore per più requisiti

Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore di autorizzazioni è in grado di gestire tre diversi tipi di requisiti:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Il codice precedente attraversa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene requisiti non contrassegnati come riusciti. Per un `ReadPermission` requisito, l'utente deve essere un proprietario o uno sponsor per accedere alla risorsa richiesta. Nel caso di un requisito `EditPermission` o `DeletePermission`, deve essere un proprietario per accedere alla risorsa richiesta.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registrazione del gestore

I gestori vengono registrati nella raccolta di servizi durante la configurazione. Ad esempio:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Il codice precedente registra `MinimumAgeHandler` come singleton richiamando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. I gestori possono essere registrati usando una qualsiasi delle [durate dei servizi](xref:fundamentals/dependency-injection#service-lifetimes)predefinite.

## <a name="what-should-a-handler-return"></a>Che cosa deve restituire un gestore?

Si noti che il metodo `Handle` nell' [esempio di gestore](#security-authorization-handler-example) non restituisce alcun valore. Come viene indicato lo stato di esito positivo o negativo?

* Un gestore indica l'esito positivo chiamando `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stato convalidato correttamente.

* Un gestore non deve gestire gli errori in modo generale, perché gli altri gestori per lo stesso requisito possono avere esito positivo.

* Per garantire un errore, anche se gli altri gestori di requisiti hanno esito positivo, chiamare `context.Fail`.

Se un gestore chiama `context.Succeed` o `context.Fail`, tutti gli altri gestori vengono ancora chiamati. Questo consente ai requisiti di produrre effetti collaterali, ad esempio la registrazione, che si verifica anche se un altro gestore ha correttamente convalidato o non ha superato un requisito. Se impostato su `false`, la proprietà [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (disponibile in ASP.NET Core 1,1 e versioni successive) esegue il cortocircuito dell'esecuzione dei gestori quando viene chiamato `context.Fail`. `InvokeHandlersAfterFailure` viene impostato il valore predefinito `true`, nel qual caso vengono chiamati tutti i gestori.

> [!NOTE]
> I gestori di autorizzazione vengono chiamati anche se l'autenticazione ha esito negativo.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Perché si vogliono usare più gestori per un requisito?

Nei casi in cui si desidera che la valutazione sia basata su **o** , implementare più gestori per un singolo requisito. Ad esempio, Microsoft dispone di porte che si aprono solo con le schede chiave. Se si lascia la scheda chiave a casa, il receptionist stampa un adesivo temporaneo e apre lo sportello. In questo scenario si avrà un singolo requisito, *BuildingEntry*, ma più gestori, ognuno di essi esaminando un singolo requisito.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Assicurarsi che entrambi i gestori siano [registrati](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Se uno dei due gestori ha esito positivo quando un criterio valuta il `BuildingEntryRequirement`, la valutazione dei criteri ha esito positivo.

## <a name="using-a-func-to-fulfill-a-policy"></a>Uso di un Func per soddisfare un criterio

In alcuni casi è possibile che l'evasione di un criterio sia semplice da esprimere nel codice. È possibile specificare un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il generatore di criteri di `RequireAssertion`.

Ad esempio, il `BadgeEntryHandler` precedente potrebbe essere riscritto come segue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Accesso al contesto della richiesta MVC nei gestori

Il metodo `HandleRequirementAsync` implementato in un gestore autorizzazioni presenta due parametri: un `AuthorizationHandlerContext` e il `TRequirement` che si sta gestendo. I Framework come MVC o Jabbr sono liberi di aggiungere qualsiasi oggetto alla proprietà `Resource` nel `AuthorizationHandlerContext` per passare informazioni aggiuntive.

Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nella proprietà `Resource`. Questa proprietà consente di accedere a `HttpContext`, `RouteData`e tutto il resto fornito da MVC e Razor Pages.

L'uso della proprietà `Resource` è specifico del Framework. L'uso delle informazioni nella proprietà `Resource` limita i criteri di autorizzazione a specifici Framework. È necessario eseguire il cast della proprietà `Resource` usando la parola chiave `is`, quindi verificare che il cast abbia avuto esito positivo per assicurarsi che il codice non si arresti in modo anomalo con un `InvalidCastException` quando viene eseguito in altri Framework:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end