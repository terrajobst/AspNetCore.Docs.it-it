---
title: Eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0
author: scottaddie
description: Questo articolo illustra i passaggi più comuni per la migrazione di ASP.NET Core 1.x autenticazione e identità ad ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892208"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0

Dal [Scott Addie](https://github.com/scottaddie) e [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2.0 include un nuovo modello per l'autenticazione e [identità](xref:security/authentication/identity) che semplifica la configurazione mediante i servizi. Applicazioni ASP.NET Core 1.x che usano l'autenticazione o identità possono essere aggiornate per usare il nuovo modello, come indicato di seguito.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Servizi e Middleware di autenticazione

Nei progetti di 1.x, viene configurata l'autenticazione tramite middleware. Per ogni schema di autenticazione che si vuole supportare, viene richiamato un metodo di middleware.

Nell'esempio seguente 1.x consente di configurare l'autenticazione di Facebook con l'identità in *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

Nei 2.0 progetti, l'autenticazione viene configurata tramite i servizi. Ogni schema di autenticazione è registrato nel `ConfigureServices` metodo di *Startup.cs*. Il `UseIdentity` metodo viene sostituito con `UseAuthentication`.

Nell'esempio seguente 2.0 consente di configurare l'autenticazione di Facebook con l'identità in *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

Il `UseAuthentication` metodo aggiunge un componente middleware di autenticazione single che è responsabile per l'autenticazione automatica e la gestione delle richieste di autenticazione remota. Sostituisce tutti i componenti middleware singole con un componente del middleware comune.

Di seguito sono 2.0 istruzioni relative alla migrazione per ogni schema di autenticazione principale.

### <a name="cookie-based-authentication"></a>Autenticazione basata su cookie

Selezionare una delle due opzioni seguenti e apportare le modifiche necessarie nel *Startup.cs*:

1. Utilizzo dei cookie con Identity
    - Sostituire `UseIdentity` con `UseAuthentication` nel `Configure` metodo:

        ```csharp
        app.UseAuthentication();
        ```

    - Richiama il `AddIdentity` metodo nella `ConfigureServices` metodo per aggiungere i servizi di autenticazione del cookie.
    - Facoltativamente, richiamare il `ConfigureApplicationCookie` o `ConfigureExternalCookie` metodo nella `ConfigureServices` metodo per modificare le impostazioni di cookie di identità.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Uso dei cookie senza Identity
    - Sostituire il `UseCookieAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Richiama il `AddAuthentication` e `AddCookie` metodi nel `ConfigureServices` metodo:

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a>Autenticazione della connessione JWT

Apportare le modifiche seguenti nel *Startup.cs*:
- Sostituire il `UseJwtBearerAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiama il `AddJwtBearer` metodo nel `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Questo frammento di codice non Usa identità, in modo che lo schema predefinito deve essere impostato passando `JwtBearerDefaults.AuthenticationScheme` per il `AddAuthentication` (metodo).

### <a name="openid-connect-oidc-authentication"></a>Autenticazione di OpenID Connect (OIDC)

Apportare le modifiche seguenti nel *Startup.cs*:

- Sostituire il `UseOpenIdConnectAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiama il `AddOpenIdConnect` metodo nel `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a>Autenticazione Facebook

Apportare le modifiche seguenti nel *Startup.cs*:
- Sostituire il `UseFacebookAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiama il `AddFacebook` metodo nel `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Autenticazione Google

Apportare le modifiche seguenti nel *Startup.cs*:
- Sostituire il `UseGoogleAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiama il `AddGoogle` metodo nel `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Autenticazione dell'Account Microsoft

Apportare le modifiche seguenti nel *Startup.cs*:
- Sostituire il `UseMicrosoftAccountAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiama il `AddMicrosoftAccount` metodo nel `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Autenticazione Twitter

Apportare le modifiche seguenti nel *Startup.cs*:
- Sostituire il `UseTwitterAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiama il `AddTwitter` metodo nel `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Schemi di autenticazione predefinito di impostazione

Nella versione 1.x, la `AutomaticAuthenticate` e `AutomaticChallenge` delle proprietà delle [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe di base sono state deve essere impostata su uno schema di autenticazione single. Si è verificato alcun efficace per applicare questo comportamento.

2.0, queste due proprietà sono stati rimossi come proprietà nelle singole `AuthenticationOptions` istanza. Può essere configurati nel `AddAuthentication` chiamata al metodo all'interno di `ConfigureServices` metodo di *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Nel frammento di codice precedente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").

In alternativa, usare una versione di overload di `AddAuthentication` metodo per impostare più di una proprietà. Nell'esempio seguente il metodo di overload, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme`. Lo schema di autenticazione in alternativa può essere specificato all'interno di singoli `[Authorize]` attributi o i criteri di autorizzazione.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Definire uno schema predefinito in 2.0 se viene soddisfatta una delle condizioni seguenti:
- Si desidera che l'utente di essere connessi automaticamente
- Si utilizza il `[Authorize]` i criteri di autorizzazione o di attributo senza specificare gli schemi

Un'eccezione a questa regola è la `AddIdentity` (metodo). Questo metodo aggiunge i cookie per l'utente e imposta l'impostazione predefinita l'autenticazione e stimolare gli schemi per il cookie dell'applicazione `IdentityConstants.ApplicationScheme`. Imposta inoltre lo schema di accesso predefinito per il cookie esterno `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Usare le estensioni di autenticazione HttpContext

Il `IAuthenticationManager` interfaccia è il punto di ingresso principale nel sistema di autenticazione 1.x. È stato sostituito con un nuovo set di `HttpContext` metodi di estensione di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.

Progetti di riferimento, ad esempio, 1.x un `Authentication` proprietà:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Nei 2.0 progetti, importare il `Microsoft.AspNetCore.Authentication` dello spazio dei nomi ed eliminare il `Authentication` riferimenti di proprietà:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>L'autenticazione di Windows (http. sys / IISIntegration)

Esistono due varianti di autenticazione di Windows:
1. L'host consente solo agli utenti autenticati
2. L'host consente sia anonimo e gli utenti autenticati

Non è interessata dalle 2.0 modifiche prima variante descritta in precedenza.

La seconda variante descritta in precedenza è interessata dalle modifiche 2.0. Ad esempio, si potrebbero essere che consente agli utenti anonimi nella tua app in IIS o [HTTP. sys](xref:fundamentals/servers/httpsys) utenti ma per l'autorizzazione a livello di Controller di livello. In questo scenario, impostare lo schema predefinito `IISDefaults.AuthenticationScheme` nella `Startup.ConfigureServices` metodo:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Impossibile impostare lo schema predefinito di conseguenza impedisce la richiesta di autorizzazione per fare in modo da in esecuzione.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Istanze IdentityCookieOptions

Un effetto collaterale delle 2.0 modifiche è l'opzione per l'utilizzo denominate opzioni anziché le istanze delle opzioni cookie. La possibilità di personalizzare i nomi di schema di cookie di identità viene rimosso.

Progetti di 1.x, ad esempio, usare [inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per passare un `IdentityCookieOptions` parametri in *AccountController.cs*. Lo schema di autenticazione esterni cookie è accessibile dall'istanza fornita:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

L'inserimento del costruttore menzionati in precedenza non è più necessario nei 2.0 progetti e `_externalCookieScheme` campo può essere eliminato:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

Il `IdentityConstants.ExternalScheme` costante può essere utilizzata direttamente:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Aggiungere le proprietà di navigazione IdentityUser POCO

Le proprietà di navigazione di Entity Framework (EF) Core della base `IdentityUser` sono state rimosse POCO (Plain Old CLR Object). Se il progetto di 1.x Usa queste proprietà, aggiungerli manualmente al progetto 2.0:

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

Per evitare duplicate chiavi esterne durante l'esecuzione di migrazioni EF Core, aggiungere il codice seguente per il `IdentityDbContext` della classe `OnModelCreating` metodo (dopo il `base.OnModelCreating();` chiamare):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a>Sostituire GetExternalAuthenticationSchemes

Metodo sincrono `GetExternalAuthenticationSchemes` è stato rimosso a favore di una versione asincrona. i progetti di 1.x avere il codice seguente *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Questo metodo viene visualizzato nella *cshtml* troppo:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

Nei 2.0 progetti, usare il `GetExternalAuthenticationSchemesAsync` metodo:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

In *cshtml*, il `AuthenticationScheme` proprietà a cui accede nel `foreach` ciclo diventa `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Modifica della proprietà ManageLoginsViewModel

Oggetto `ManageLoginsViewModel` oggetto viene utilizzato nel `ManageLogins` azione di *ManageController.cs*. Nei progetti di 1.x, l'oggetto `OtherLogins` proprietà tipo restituito è `IList<AuthenticationDescription>`. Il tipo restituito richiede un'importazione di `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Nei 2.0 progetti, il tipo restituito viene impostato su `IList<AuthenticationScheme>`. Questo nuovo tipo restituito è necessario sostituire il `Microsoft.AspNetCore.Http.Authentication` importare con un `Microsoft.AspNetCore.Authentication` importare.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

Per ulteriori informazioni e discussione, vedere la [discussione per 2.0 Auth](https://github.com/aspnet/Security/issues/1338) problema in GitHub.
