---
title: Autorizzare con uno schema specifico in ASP.NET Core
author: rick-anderson
description: In questo articolo viene illustrato come limitare l'identità a uno schema specifico quando si utilizzano più metodi di autenticazione.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: a3be2b8171c146beef7e62c8f7e55883ca5dc687
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661819"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autorizzare con uno schema specifico in ASP.NET Core

IIn alcuni scenari, come applicazioni a pagina singola (SPAs), è frequente l'utilizzo di più metodi di autenticazione. Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript. In alcuni casi, l'app può avere più istanze di un gestore di autenticazione. Ad esempio, due gestori di cookie in cui uno contiene un'identità di base e uno viene creato quando viene attivata un'autenticazione a più fattori (multi-factor authentication). L'autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza. Per altre informazioni sull'applicazione dell'autenticazione a più fattori quando un utente richiede una risorsa che richiede l'autenticazione a più fattori, vedere la [sezione relativa alla protezione](https://github.com/dotnet/AspNetCore.Docs/issues/15791#issuecomment-580464195)dei problemi di GitHub con l'autenticazione a

Uno schema di autenticazione viene denominato quando il servizio di autenticazione viene configurato durante l'autenticazione. Ad esempio:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

Nel codice precedente sono stati aggiunti due gestori di autenticazione: uno per i cookie e uno per il titolare.

>[!NOTE]
>La specifica dello schema predefinito comporta l'impostazione della proprietà `HttpContext.User` su tale identità. Se non si desidera questo comportamento, disabilitarlo richiamando il formato senza parametri di `AddAuthentication`.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Selezione dello schema con l'attributo autorizzi

Al momento dell'autorizzazione, l'app indica il gestore da usare. Consente di selezionare il gestore con cui l'app autorizzerà il passaggio di un elenco delimitato da virgole di schemi di autenticazione a `[Authorize]`. L'attributo `[Authorize]` specifica lo schema o gli schemi di autenticazione da utilizzare, indipendentemente dal fatto che sia configurato un valore predefinito. Ad esempio:

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

Nell'esempio precedente vengono eseguiti sia i gestori di cookie che i gestori di porta e hanno la possibilità di creare e aggiungere un'identità per l'utente corrente. Specificando solo un singolo schema, viene eseguito il gestore corrispondente.

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

Nel codice precedente viene eseguito solo il gestore con lo schema "Bearer". Eventuali identità basate su cookie vengono ignorate.

## <a name="selecting-the-scheme-with-policies"></a>Selezione dello schema con i criteri

Se si preferisce specificare gli schemi desiderati nel [criterio](xref:security/authorization/policies), è possibile impostare la raccolta `AuthenticationSchemes` quando si aggiungono i criteri:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

Nell'esempio precedente, il criterio "Over18" viene eseguito solo con l'identità creata dal gestore "Bearer". Usare il criterio impostando la proprietà `Policy` dell'attributo `[Authorize]`:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Usare più schemi di autenticazione

Alcune app potrebbero dover supportare più tipi di autenticazione. Ad esempio, l'app potrebbe autenticare gli utenti da Azure Active Directory e da un database degli utenti. Un altro esempio è un'app che autentica gli utenti da Active Directory Federation Services e Azure Active Directory B2C. In questo caso, l'app deve accettare una bearer token JWT da diverse autorità emittenti.

Aggiungere tutti gli schemi di autenticazione che si desidera accettare. Ad esempio, il codice seguente in `Startup.ConfigureServices` aggiunge due schemi di autenticazione di JWT Bearer con emittenti differenti:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> Con lo schema di autenticazione predefinito `JwtBearerDefaults.AuthenticationScheme`viene registrata una sola autenticazione di JWT Bearer. È necessario registrare un'autenticazione aggiuntiva con uno schema di autenticazione univoco.

Il passaggio successivo consiste nell'aggiornare i criteri di autorizzazione predefiniti affinché accettino entrambi gli schemi di autenticazione. Ad esempio:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

Quando viene eseguito l'override dei criteri di autorizzazione predefiniti, è possibile usare l'attributo `[Authorize]` nei controller. Il controller accetta quindi le richieste con JWT emesso dal primo o dal secondo emittente.

::: moniker-end
