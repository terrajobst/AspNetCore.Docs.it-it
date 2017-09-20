---
title: "Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core"
author: rick-anderson
description: Una spiegazione dell'utilizzo dell'autenticazione cookie senza ASP.NET Identity Core
keywords: ASP.NET Core, i cookie
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: b728c3d62b59f28f1d020b6f3732918a1fcdf4eb
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1. x fornisce il cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) che serializza un'entità utente in un cookie crittografato e nelle richieste successive convalida quindi il cookie, ricrea l'entità e lo assegna al `HttpContext.User` proprietà . Se si desidera fornire schermate di accesso e i database utente, è possibile utilizzare il middleware del cookie come funzionalità autonoma.

Una modifica importante in ASP.NET Core 2. x è che il middleware del cookie è assente. Invece di `UseAuthentication` chiamata al metodo nel `Configure` metodo *Startup.cs* aggiunge AuthenticationMiddleware che imposta il `HttpContext.User` proprietà.

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>Aggiunta e configurazione

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Completare i passaggi seguenti:

- Se non si utilizza il `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), installare la versione 2.0 del `Microsoft.AspNetCore.Authentication.Cookies` pacchetto NuGet nel progetto.

- Richiamare il `UseAuthentication` metodo il `Configure` metodo il *Startup.cs* file:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddAuthentication` e `AddCookie` metodi il `ConfigureServices` metodo il *Startup.cs* file:

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie(options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Completare i passaggi seguenti:

- Installare il `Microsoft.AspNetCore.Authentication.Cookies` pacchetto NuGet nel progetto. Questo pacchetto contiene middleware del cookie.

- Aggiungere le seguenti righe al `Configure` metodo nel *Startup.cs* file prima di `app.UseMvc()` istruzione:

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

I frammenti di codice precedenti per configurare alcune o tutte le opzioni seguenti:

* `AccessDeniedPath`-Si tratta del percorso relativo in cui le richieste di reindirizzamento quando un utente tenta di accedere a una risorsa ma non supera [criteri di autorizzazione](xref:security/authorization/policies#security-authorization-policies-based) per tale risorsa.

* `AuthenticationScheme`-Si tratta di un valore con cui è noto uno schema particolare cookie di autenticazione. Ciò è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [limitare l'autorizzazione a un'istanza](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).

* `AutomaticAuthenticate`-Questo flag è rilevante solo per ASP.NET Core 1. x. Indica che l'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.

* `AutomaticChallenge`-Questo flag è rilevante solo per ASP.NET Core 1. x. Indica che l'autenticazione dei cookie 1. x deve reindirizzare il browser per il `LoginPath` o `AccessDeniedPath` quando autorizzazione ha esito negativo.

* `LoginPath`-Questo è il percorso relativo in cui le richieste di reindirizzamento quando un utente tenta di accedere a una risorsa ma non è stato autenticato.

[Altre opzioni](xref:security/authentication/cookie#security-authentication-cookie-options) includono la possibilità di impostare l'autorità emittente per tutte le attestazioni di autenticazione dei cookie crea, Elimina il nome del cookie di autenticazione, il dominio per il cookie e varie proprietà di sicurezza nel cookie. Per impostazione predefinita, l'autenticazione dei cookie utilizza le opzioni di sicurezza appropriate per i cookie che crea, ad esempio:
- L'impostazione del contrassegno HttpOnly per impedire l'accesso di cookie in JavaScript sul lato client
- Limitare il cookie a HTTPS, se una richiesta è stato trasferito su HTTPS

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>Creazione di un cookie di identità

Per creare un cookie che contiene le informazioni sull'utente, è necessario costruire un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) che contiene le informazioni che si desidera essere serializzati nel cookie. Dopo aver adatto `ClaimsPrincipal` di oggetto, chiamare il seguente all'interno del metodo del controller:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

Questo crea un cookie crittografato e lo aggiunge alla risposta corrente. Il `AuthenticationScheme` specificato durante [configurazione](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) deve essere utilizzato quando si chiama `SignInAsync`.

Dietro le quinte, la crittografia utilizzata è ASP.NET Core [la protezione dei dati](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema. Se si trovano in più computer, carico bilanciamento del carico o utilizzare una web farm, è quindi necessario [configurare la protezione dati](xref:security/data-protection/configuration/overview#data-protection-configuring) come utilizzare la stessa chiave anello e identificatore dell'applicazione.

## <a name="signing-out"></a>La disconnessione

Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare il seguente all'interno del controller:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>Reazione alle modifiche di back-end

>[!WARNING]
> Dopo aver creato un cookie principale, diventa l'unica origine di identità. Anche se si disabilita un utente nei sistemi back-end, l'autenticazione dei cookie non ha alcuna conoscenza di questo oggetto e un utente rimane connesso, purché i cookie è valido.

L'autenticazione dei cookie viene fornita una serie di eventi nella relativa classe di opzione. Il `ValidateAsync()` evento consente di intercettare ed eseguire l'override di convalida dell'identità del cookie.

Prendere in considerazione un database utente di back-end che può contenere una colonna "LastChanged". Per invalidare un cookie quando viene modificato il database, è necessario innanzitutto, quando [creazione il cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), aggiungere un'attestazione "LastChanged" contenente il valore corrente. Quando viene modificato il database, è necessario aggiornare il valore "LastChanged".

Per implementare un override per il `ValidateAsync()` evento, è necessario scrivere un metodo con la firma seguente:

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

ASP.NET Identity Core implementa questo controllo come parte del relativo `SecurityStampValidator`. Un esempio è simile al seguente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Questo sarebbe quindi necessario collegare durante la registrazione del servizio nel cookie di `ConfigureServices` metodo *Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie(options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Questo sarebbe quindi necessario collegare durante la configurazione dell'autenticazione nel cookie di `Configure` metodo *Startup.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Si consideri l'esempio in cui è stato aggiornato il proprio nome &mdash; una decisione che non influisce sulla sicurezza in alcun modo. Se si desidera aggiornare distruttivo l'entità utente, è possibile chiamare `context.ReplacePrincipal()` e impostare il `context.ShouldRenew` proprietà `true`.

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>Controllo delle opzioni di cookie

Il [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) classe dotata di varie opzioni di configurazione per ottimizzare i cookie viene creati.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

2. x unifica le API utilizzate per i cookie di configurazione di ASP.NET Core. Il 1. x API sono state contrassegnate come obsolete e un nuovo `Cookie` proprietà di tipo `CookieBuilder` è stato introdotto nel `CookieAuthenticationOptions` classe. È consigliabile eseguire la migrazione a 2. x API.

* `ClaimsIssuer`è l'autorità emittente da utilizzare per il [dell'autorità di certificazione](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato per l'autenticazione dei cookie.

* `CookieBuilder.Domain`è il nome di dominio a cui il cookie viene servito. Per impostazione predefinita, questo è il nome host che è stata inviata la richiesta. Il browser opera solo il cookie a un nome host corrispondente. Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio. Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`e così via.

* `CookieBuilder.HttpOnly`flag che indica se il cookie deve essere accessibile solo ai server. L'impostazione predefinita è `true`. Se si modifica questo valore può aprire l'applicazione per il furto di cookie l'applicazione deve disporre di un bug Cross-Site Scripting.

* `CookieBuilder.Path`Consente di isolare le applicazioni eseguono lo stesso nome host. Se si dispone di un'app in esecuzione in `/app1` per limitare i cookie per essere inviati solo a tale applicazione, quindi è necessario impostare il `CookiePath` proprietà `/app1`. In questo modo, il cookie è disponibile solo per le richieste a `/app1` o qualsiasi elemento in esso contenuti.

* `CookieBuilder.SameSite`indica se il browser deve consentire i cookie da collegare alle richieste nello stesso sito o tra siti. L'impostazione predefinita è `SameSiteMode.Lax`.

* `CookieBuilder.SecurePolicy`flag che indica se il cookie creato deve essere limitato a HTTPS, HTTP o HTTPS o lo stesso protocollo della richiesta. L'impostazione predefinita è `SameAsRequest`.

* `ExpireTimeSpan`è il `TimeSpan` dopo che il cookie scade. Viene aggiunto per la data e ora correnti per creare la data di scadenza del cookie.

* `SlidingExpiration`flag che indica se la data di scadenza del cookie viene reimpostato quando più di metà del `ExpireTimeSpan` trascorso l'intervallo. La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`. Un [ora di scadenza assoluto](xref:security/authentication/cookie#security-authentication-absolute-expiry) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`. Una scadenza assoluta può migliorare la sicurezza dell'applicazione per limitare la quantità di tempo per cui il cookie di autenticazione è valido.

Un esempio di utilizzo `CookieAuthenticationOptions` nel `ConfigureServices` metodo *Startup.cs* segue:

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`è l'autorità emittente da utilizzare per il [dell'autorità di certificazione](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal middleware.

* `CookieDomain`è il nome di dominio a cui il cookie viene servito. Per impostazione predefinita, questo è il nome host che è stata inviata la richiesta. Il browser opera solo il cookie a un nome host corrispondente. Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio. Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`e così via.

* `CookieHttpOnly`flag che indica se il cookie deve essere accessibile solo ai server. L'impostazione predefinita è `true`. Se si modifica questo valore può aprire l'applicazione per il furto di cookie l'applicazione deve disporre di un bug Cross-Site Scripting.

* `CookiePath`Consente di isolare le applicazioni eseguono lo stesso nome host. Se si dispone di un'app in esecuzione in `/app1` per limitare i cookie per essere inviati solo a tale applicazione, quindi è necessario impostare il `CookiePath` proprietà `/app1`. In questo modo, il cookie è disponibile solo per le richieste a `/app1` o qualsiasi elemento in esso contenuti.

* `CookieSecure`flag che indica se il cookie creato deve essere limitato a HTTPS, HTTP o HTTPS o lo stesso protocollo della richiesta. L'impostazione predefinita è `SameAsRequest`.

* `ExpireTimeSpan`è il `TimeSpan` dopo che il cookie scade. Viene aggiunto per la data e ora correnti per creare la data di scadenza del cookie.

* `SlidingExpiration`flag che indica se la data di scadenza del cookie viene reimpostato quando più di metà del `ExpireTimeSpan` trascorso l'intervallo. La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`. Un [ora di scadenza assoluto](xref:security/authentication/cookie#security-authentication-absolute-expiry) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`. Una scadenza assoluta può migliorare la sicurezza dell'applicazione per limitare la quantità di tempo per cui il cookie di autenticazione è valido.

Un esempio di utilizzo `CookieAuthenticationOptions` nel `Configure` metodo *Startup.cs* segue:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a>I cookie permanenti e ora di scadenza assoluto

È la scadenza del cookie per rendere persistenti le sessioni del browser e si desidera una scadenza assoluta per l'identità e il cookie il trasporto. Questo tipo di persistenza deve essere abilitata solo con consenso esplicito, tramite un controllo checkbox "Memorizza dati" su account di accesso o un meccanismo simile. È possibile eseguire queste operazioni utilizzando il `AuthenticationProperties` parametro il `SignInAsync` metodo chiamato quando [un'identità di firma e la creazione del cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie). Ad esempio:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Il `AuthenticationProperties` (classe), utilizzato nel frammento di codice precedente, si trova nel `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Il `AuthenticationProperties` (classe), utilizzato nel frammento di codice precedente, si trova nel `Microsoft.AspNetCore.Http.Authentication` dello spazio dei nomi.

---

Frammento di codice precedente crea un'identità e i cookie corrispondente che resta tramite chiusure browser. Qualsiasi variabile configurate in precedenza tramite le impostazioni di scadenza [opzioni del cookie](xref:security/authentication/cookie#security-authentication-cookie-options) sono comunque supportati. Se il cookie scade al contempo il browser viene chiuso, il browser cancellato quando viene riavviato.

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

Frammento di codice precedente crea un'identità e i cookie corrispondente che la durata è di 20 minuti. Questo ignora eventuali impostazioni di scadenza variabile configurati in precedenza tramite [opzioni del cookie](xref:security/authentication/cookie#security-authentication-cookie-options).

Il `ExpiresUtc` e `IsPersistent` si escludono a vicenda.
