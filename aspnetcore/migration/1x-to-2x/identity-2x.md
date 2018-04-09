---
title: Eseguire la migrazione di autenticazione e identità per ASP.NET 2.0 Core
author: scottaddie
description: Questo articolo vengono illustrati i passaggi più comuni per la migrazione ASP.NET Core 1. x autenticazione e identità per ASP.NET 2.0 Core.
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 16369a14dbe97778724632317a82e11de5a8faed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Eseguire la migrazione di autenticazione e identità per ASP.NET 2.0 Core

Da [Scott Addie](https://github.com/scottaddie) e [Hao Kung](https://github.com/HaoK)

Componenti di base di ASP.NET 2.0 è un nuovo modello per l'autenticazione e [identità](xref:security/authentication/identity) che semplifica la configurazione mediante i servizi. Applicazioni ASP.NET Core 1. x che utilizzano l'autenticazione o identità possono essere aggiornate per utilizzare il nuovo modello, come descritto di seguito.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Middleware di autenticazione e i servizi
In progetti di 1. x, è configurata l'autenticazione tramite il middleware. Viene richiamato un metodo di middleware per ogni schema di autenticazione che si desidera supportare.

Nell'esempio seguente di 1. x viene configurata l'autenticazione di Facebook con identità nella *Startup.cs*:

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

Nei progetti a 2.0, l'autenticazione viene configurata tramite i servizi. Ogni schema di autenticazione è registrato nel `ConfigureServices` metodo *Startup.cs*. Il `UseIdentity` (metodo) viene sostituito con `UseAuthentication`.

Nell'esempio seguente viene 2.0 viene configurata l'autenticazione di Facebook con identità nella *Startup.cs*:

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

Il `UseAuthentication` metodo aggiunge un componente di middleware di autenticazione single che è responsabile per l'autenticazione automatica e la gestione delle richieste di autenticazione remota. Sostituisce tutti i componenti middleware singoli con un componente del middleware comune.

Di seguito sono 2.0 istruzioni relative alla migrazione per ogni schema di autenticazione principale.

### <a name="cookie-based-authentication"></a>Autenticazione basata su cookie
Selezionare una delle due opzioni seguenti e apportare le modifiche necessarie in *Startup.cs*:

1. Utilizzare i cookie con identità
    - Sostituire `UseIdentity` con `UseAuthentication` nel `Configure` metodo:

        ```csharp
        app.UseAuthentication();
        ```

    - Richiamare il `AddIdentity` metodo il `ConfigureServices` metodo per aggiungere i servizi di autenticazione del cookie.
    - Facoltativamente, richiamare il `ConfigureApplicationCookie` o `ConfigureExternalCookie` metodo il `ConfigureServices` metodo per modificare le impostazioni dei cookie di identità.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Utilizzare i cookie senza identità
    - Sostituire il `UseCookieAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - Richiamare il `AddAuthentication` e `AddCookie` metodi di `ConfigureServices` metodo:

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
Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire il `UseJwtBearerAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddJwtBearer` metodo il `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Questo frammento di codice non usa l'identità, pertanto lo schema predefinito deve essere impostato passando `JwtBearerDefaults.AuthenticationScheme` per il `AddAuthentication` metodo.

### <a name="openid-connect-oidc-authentication"></a>Autenticazione di OpenID Connect (OIDC)
Apportare le modifiche seguenti in *Startup.cs*:

- Sostituire il `UseOpenIdConnectAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddOpenIdConnect` metodo il `ConfigureServices` metodo:

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

### <a name="facebook-authentication"></a>autenticazione di Facebook
Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire il `UseFacebookAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddFacebook` metodo il `ConfigureServices` metodo:
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Autenticazione di Google
Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire il `UseGoogleAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddGoogle` metodo il `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Autenticazione dell'Account Microsoft
Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire il `UseMicrosoftAccountAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddMicrosoftAccount` metodo il `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Autenticazione di Twitter
Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire il `UseTwitterAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il `AddTwitter` metodo il `ConfigureServices` metodo:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>L'impostazione di schemi di autenticazione predefinito
In 1. x, il `AutomaticAuthenticate` e `AutomaticChallenge` le proprietà del [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) sono stati deve essere impostata su uno schema di autenticazione singola classe di base. Si è verificato non è possibile applicare questo comportamento.

In 2.0 sono state rimosse queste due proprietà come proprietà per il singolo `AuthenticationOptions` istanza. Può essere configurati nel `AddAuthentication` chiamata al metodo all'interno di `ConfigureServices` metodo *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Nel frammento di codice precedente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").

In alternativa, utilizzare una versione di overload di `AddAuthentication` per impostare più di una proprietà. Nell'esempio seguente il metodo di overload, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme`. In alternativa, lo schema di autenticazione può essere specificato all'interno di singoli `[Authorize]` attributi o i criteri di autorizzazione.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Definire uno schema predefinito in 2.0 se viene soddisfatta una delle condizioni seguenti:
- Si desidera che l'utente di essere connessi automaticamente
- Utilizzare il `[Authorize]` i criteri di autorizzazione o di attributo senza specificare gli schemi

Un'eccezione a questa regola è la `AddIdentity` metodo. Questo metodo aggiunge i cookie per l'utente e imposta il valore predefinito di autenticare e richiesta di schemi per il cookie dell'applicazione `IdentityConstants.ApplicationScheme`. Inoltre, imposta lo schema predefinito di accesso nel cookie esterno `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Utilizzare le estensioni di autenticazione HttpContext
Il `IAuthenticationManager` interfaccia è il punto di ingresso principale nel sistema di autenticazione di 1. x. È stato sostituito con un nuovo set di `HttpContext` metodi di estensione di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.

Progetti di riferimento, ad esempio, 1. x un `Authentication` proprietà:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Nei progetti a 2.0, importare il `Microsoft.AspNetCore.Authentication` dello spazio dei nomi ed eliminare il `Authentication` riferimenti alle proprietà:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>L'autenticazione di Windows (HTTP.sys / IISIntegration)
Esistono due varianti dell'autenticazione di Windows:
1. L'host consente solo agli utenti autenticati
2. L'host consente sia anonimi e gli utenti autenticati

La prima variante descritta in precedenza non è influenzata dalle 2.0 modifiche.

Descritto in precedenza mentre la seconda è interessata dalle 2.0 modifiche. Ad esempio, si possono consentire agli utenti anonimi all'applicazione in IIS o [HTTP.sys](xref:fundamentals/servers/weblistener) livelli di autorizzazione ma gli utenti a livello di Controller. In questo scenario, impostare lo schema predefinito `IISDefaults.AuthenticationScheme` nel `ConfigureServices` metodo *Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Impossibile impostare lo schema predefinito impedisce conseguenza richiesta authorize di incentivare l'utilizzo.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Istanze di IdentityCookieOptions
Un effetto collaterale delle 2.0 modifiche è il passaggio a denominato opzioni anziché le istanze di opzioni di cookie. La possibilità di personalizzare i nomi di schema di cookie di identità viene rimossa.

Ad esempio, 1. x progetti utilizzano [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per passare un `IdentityCookieOptions` parametro in *AccountController.cs*. Lo schema di autenticazione esterno cookie è accessibile dall'istanza fornita:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

L'inserimento di un costruttore menzionati in precedenza non è necessaria nei progetti di 2.0 e `_externalCookieScheme` campo può essere eliminato:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

Il `IdentityConstants.ExternalScheme` costante può essere utilizzata direttamente:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Aggiungere le proprietà di navigazione IdentityUser POCO
Le proprietà di navigazione di base di Entity Framework (EF) della base `IdentityUser` sono state rimosse POCO (Plain precedente CLR Object). Se il progetto di 1. x utilizza queste proprietà, aggiungerle manualmente al progetto 2.0:

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

Per evitare duplicate chiavi esterne durante l'esecuzione di migrazioni di Entity Framework Core, aggiungere il comando seguente per il `IdentityDbContext` classe `OnModelCreating` (metodo) (dopo il `base.OnModelCreating();` chiamare):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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
Il metodo sincrono `GetExternalAuthenticationSchemes` a favore di una versione asincrona è stata rimossa. 1. x progetti presentano nel codice seguente *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Questo metodo viene visualizzato *cshtml* troppo:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

Nei 2.0 progetti, usare il `GetExternalAuthenticationSchemesAsync` metodo:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

In *cshtml*, `AuthenticationScheme` proprietà accessibile nel `foreach` ciclo diventa `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Modifica della proprietà ManageLoginsViewModel
Oggetto `ManageLoginsViewModel` oggetto viene utilizzato nel `ManageLogins` azione di *ManageController.cs*. In del 1. x progetti, l'oggetto `OtherLogins` proprietà tipo restituito è `IList<AuthenticationDescription>`. Il tipo restituito richiede un'importazione di `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Nei progetti a 2.0, il tipo restituito viene modificato per `IList<AuthenticationScheme>`. Questo nuovo tipo restituito è sostituire il `Microsoft.AspNetCore.Http.Authentication` importare con un `Microsoft.AspNetCore.Authentication` importare.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Risorse aggiuntive
Per ulteriori dettagli e informazioni, vedere il [discussione per autenticazione 2.0](https://github.com/aspnet/Security/issues/1338) problema su GitHub.
