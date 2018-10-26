---
title: Autorizzare con uno schema specifico in ASP.NET Core
author: rick-anderson
description: Questo articolo illustra come limitare l'identità per uno schema specifico quando si lavora con più metodi di autenticazione.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: fbe9f32e01a214f41b5a6e9f43e8fdee5fc612df
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089396"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autorizzare con uno schema specifico in ASP.NET Core

IIn alcuni scenari, come applicazioni a pagina singola (SPAs), è frequente l'utilizzo di più metodi di autenticazione. Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript. In alcuni casi, l'app può avere più istanze di un gestore di autenticazione. Ad esempio, due gestori di cookie in cui uno contenente un'identità di base e uno viene creato quando è stata attivata l'autenticazione a più fattori (MFA). Autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede sicurezza aggiuntiva.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Uno schema di autenticazione denominato quando il servizio di autenticazione viene configurato durante l'autenticazione. Ad esempio:

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

Nel codice precedente, sono state aggiunte due gestori di autenticazione: uno per i cookie e uno per la connessione.

>[!NOTE]
>Specificando lo schema predefinito viene generato il `HttpContext.User` proprietà viene impostata su tale identità. Se non è necessario tale comportamento, disattivarlo richiamando la forma senza parametri di `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Gli schemi di autenticazione sono denominati quando il middleware di autenticazione vengono configurato durante l'autenticazione. Ad esempio:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

Nel codice precedente, sono state aggiunte due middleware di autenticazione: uno per i cookie e uno per la connessione.

>[!NOTE]
>Specificando lo schema predefinito viene generato il `HttpContext.User` proprietà viene impostata su tale identità. Se tale comportamento non è necessario, disabilitarla impostando il `AuthenticationOptions.AutomaticAuthenticate` proprietà `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Selezionare lo schema con l'attributo Authorize

Al momento di autorizzazione, l'app indica il gestore da utilizzare. Selezionare il gestore con cui l'app verrà autorizzare passando un elenco delimitato da virgole di schemi di autenticazione `[Authorize]`. Il `[Authorize]` attributo specifica lo schema di autenticazione o gli schemi da usare indipendentemente dal fatto che sia configurato un valore predefinito. Ad esempio:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

Nell'esempio precedente, gestori bearer sia il cookie eseguiti e hanno la possibilità di creare e aggiungere un'identità per l'utente corrente. Specificando un solo schema, viene eseguito il gestore corrispondente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

Nel codice precedente, viene eseguito solo il gestore con lo schema "Bearer". Qualsiasi identità basata su cookie vengono ignorate.

## <a name="selecting-the-scheme-with-policies"></a>Selezionare lo schema con i criteri

Se si preferisce specificare le combinazioni di desiderato [criteri](xref:security/authorization/policies), è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio:

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

Nell'esempio precedente, il criterio "Over18" viene eseguito solo in base all'identità creato dal gestore "Bearer". Usare i criteri impostando il `[Authorize]` dell'attributo `Policy` proprietà:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Usare più schemi di autenticazione

Alcune App potrebbe essere necessario supportare più tipi di autenticazione. Ad esempio, l'app può autenticare gli utenti da Azure Active Directory e da un database di utenti. Un altro esempio è un'app che autentica gli utenti da Active Directory Federation Services e Azure Active Directory B2C. In questo caso, l'app deve accettare un token di connessione JWT di autorità emittenti diversi.

Aggiungere tutti gli schemi di autenticazione che si vuole accettare. Ad esempio, il codice seguente `Startup.ConfigureServices` aggiunge due schemi di autenticazione bearer token JWT con emittenti diversi:

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
> Un solo l'autenticazione bearer token JWT è registrata con lo schema di autenticazione predefinito `JwtBearerDefaults.AuthenticationScheme`. L'autenticazione aggiuntiva deve essere registrato con uno schema di autenticazione univoche.

Il passaggio successivo consiste nell'aggiornare i criteri di autorizzazione predefiniti per accettare entrambi gli schemi di autenticazione. Ad esempio:

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

I criteri di autorizzazione predefinito vengano eseguito l'override, è possibile usare un semplice `[Authorize]` attributo nel controller. Quindi, il controller accetta le richieste con token JWT rilasciato dall'autorità emittente primo o secondo.

::: moniker-end
