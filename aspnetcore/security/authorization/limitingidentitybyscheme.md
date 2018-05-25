---
title: Autorizzazione con uno schema specifico in ASP.NET Core
author: rick-anderson
description: In questo articolo viene illustrato come limitare l'identità per uno schema specifico quando si lavora con più metodi di autenticazione.
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 81a01d7de8221fcb3bf90a108d9df6633ca2b696
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autorizzazione con uno schema specifico in ASP.NET Core

IIn alcuni scenari, come applicazioni a pagina singola (SPAs), è frequente l'utilizzo di più metodi di autenticazione. Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript. In alcuni casi, l'app può avere più istanze di un gestore di autenticazione. Ad esempio, due gestori di cookie in cui uno contiene un'identità di base e uno viene creato quando un'autenticazione a più fattori (MFA) è stata attivata. Autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza.

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

Nel codice precedente, sono stati aggiunti due gestori di autenticazione: uno per i cookie e uno per la connessione.

>[!NOTE]
>Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità. Se non si desidera questo comportamento, disattivarlo richiamando la forma senza parametri di `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Gli schemi di autenticazione sono denominati quando l'autenticazione middlewares vengono configurati durante l'autenticazione. Ad esempio:

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

Nel codice precedente, sono stati aggiunti due autenticazione middlewares: uno per i cookie e uno per la connessione.

>[!NOTE]
>Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità. Questo comportamento non è possibile disabilitarlo impostando il `AuthenticationOptions.AutomaticAuthenticate` proprietà `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Selezionare lo schema con l'attributo Authorize

Al momento di autorizzazione, l'app indica il gestore da utilizzare. Selezionare il gestore con cui l'app autorizzerà passando un elenco delimitato da virgole di schemi di autenticazione `[Authorize]`. Il `[Authorize]` attributo specifica lo schema di autenticazione o il schemi da utilizzare, indipendentemente dal fatto che sia stata configurata un'impostazione predefinita. Ad esempio:

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

Nell'esempio precedente, i gestori di connessione sia il cookie eseguire e la possibilità di creare e aggiungere un'identità per l'utente corrente. Specificando un unico schema, viene eseguito il gestore corrispondente.

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

Nel codice precedente, viene eseguito solo il gestore con lo schema "Bearer". Vengono ignorate le identità basate su cookie.

## <a name="selecting-the-scheme-with-policies"></a>Lo schema con i criteri di selezione

Se si preferisce specificare gli schemi desiderati in [criteri](xref:security/authorization/policies), è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio:

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

Nell'esempio precedente, il criterio "Over18" viene eseguito solo in base all'identità creata dal gestore "Bearer". Usare i criteri impostando il `[Authorize]` dell'attributo `Policy` proprietà:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
