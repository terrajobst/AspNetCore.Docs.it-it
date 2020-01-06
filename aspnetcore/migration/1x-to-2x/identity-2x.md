---
title: Eseguire la migrazione dell'autenticazione e dell'identità a ASP.NET Core 2,0
author: scottaddie
description: In questo articolo vengono illustrati i passaggi più comuni per la migrazione dell'identità e dell'autenticazione ASP.NET Core 1. x a ASP.NET Core 2,0.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: f3817fa1808c331f7e167618e3bb00d68ad08571
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355173"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Eseguire la migrazione dell'autenticazione e dell'identità a ASP.NET Core 2,0

Di [Scott Addie](https://github.com/scottaddie) e [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2,0 dispone di un nuovo modello per l'autenticazione e l' [identità](xref:security/authentication/identity) che semplifica la configurazione tramite i servizi. È possibile aggiornare le applicazioni ASP.NET Core 1. x che usano l'autenticazione o l'identità per usare il nuovo modello, come descritto di seguito.

## <a name="update-namespaces"></a>Aggiornare gli spazi dei nomi

In 1. x, le classi `IdentityRole` e `IdentityUser` sono state trovate nello spazio dei nomi `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

In 2,0 lo spazio dei nomi <xref:Microsoft.AspNetCore.Identity> è diventato il nuovo Home page per diverse classi di questo tipo. Con il codice di identità predefinito, le classi interessate includono `ApplicationUser` e `Startup`. Modificare le istruzioni `using` per risolvere i riferimenti interessati.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Middleware e servizi di autenticazione

Nei progetti di 1. x, l'autenticazione viene configurata tramite middleware. Viene richiamato un metodo middleware per ogni schema di autenticazione che si desidera supportare.

Nell'esempio 1. x seguente viene configurata l'autenticazione di Facebook con identità in *Startup.cs*:

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

Nei progetti 2,0, l'autenticazione viene configurata tramite i servizi. Ogni schema di autenticazione viene registrato nel metodo `ConfigureServices` di *Startup.cs*. Il metodo `UseIdentity` viene sostituito con `UseAuthentication`.

L'esempio 2,0 seguente configura l'autenticazione Facebook con Identity in *Startup.cs*:

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

Il metodo `UseAuthentication` aggiunge un singolo componente middleware di autenticazione, responsabile dell'autenticazione automatica e della gestione delle richieste di autenticazione remota. Sostituisce tutti i singoli componenti middleware con un singolo componente middleware comune.

Di seguito sono riportate le istruzioni di migrazione 2,0 per ogni schema di autenticazione principale.

### <a name="cookie-based-authentication"></a>Autenticazione basata su cookie

Selezionare una delle due opzioni seguenti e apportare le modifiche necessarie in *Startup.cs*:

1. Usa cookie con identità
    - Sostituire `UseIdentity` con `UseAuthentication` nel metodo `Configure`:

        ```csharp
        app.UseAuthentication();
        ```

    - Richiamare il metodo `AddIdentity` nel metodo `ConfigureServices` per aggiungere i servizi di autenticazione dei cookie.
    - Facoltativamente, richiamare il metodo `ConfigureApplicationCookie` o `ConfigureExternalCookie` nel metodo `ConfigureServices` per modificare le impostazioni del cookie di identità.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Usa cookie senza identità
    - Sostituire la chiamata al metodo `UseCookieAuthentication` nel metodo `Configure` con `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Richiamare i metodi `AddAuthentication` e `AddCookie` nel metodo `ConfigureServices`:

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

### <a name="jwt-bearer-authentication"></a>Autenticazione di JWT Bearer

Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire la chiamata al metodo `UseJwtBearerAuthentication` nel metodo `Configure` con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il metodo `AddJwtBearer` nel metodo `ConfigureServices`:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Questo frammento di codice non usa l'identità, quindi è necessario impostare lo schema predefinito passando `JwtBearerDefaults.AuthenticationScheme` al metodo `AddAuthentication`.

### <a name="openid-connect-oidc-authentication"></a>Autenticazione OpenID Connect (OIDC)

Apportare le modifiche seguenti in *Startup.cs*:

- Sostituire la chiamata al metodo `UseOpenIdConnectAuthentication` nel metodo `Configure` con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il metodo `AddOpenIdConnect` nel metodo `ConfigureServices`:

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

- Sostituire la proprietà `PostLogoutRedirectUri` nell'azione `OpenIdConnectOptions` con `SignedOutRedirectUri`:

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a>Autenticazione Facebook

Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire la chiamata al metodo `UseFacebookAuthentication` nel metodo `Configure` con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il metodo `AddFacebook` nel metodo `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Autenticazione Google

Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire la chiamata al metodo `UseGoogleAuthentication` nel metodo `Configure` con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il metodo `AddGoogle` nel metodo `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Autenticazione tramite account Microsoft

Per ulteriori informazioni sull'autenticazione account Microsoft, vedere [questo problema di GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/14455).

Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire la chiamata al metodo `UseMicrosoftAccountAuthentication` nel metodo `Configure` con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il metodo `AddMicrosoftAccount` nel metodo `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Autenticazione Twitter

Apportare le modifiche seguenti in *Startup.cs*:
- Sostituire la chiamata al metodo `UseTwitterAuthentication` nel metodo `Configure` con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Richiamare il metodo `AddTwitter` nel metodo `ConfigureServices`:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Impostazione degli schemi di autenticazione predefiniti

In 1. x, le proprietà `AutomaticAuthenticate` e `AutomaticChallenge` della classe di base [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) sono state progettate per essere impostate su un unico schema di autenticazione. Non esiste un modo efficace per applicare questa operazione.

In 2,0 queste due proprietà sono state rimosse come proprietà nella singola istanza di `AuthenticationOptions`. Possono essere configurate nella chiamata al metodo `AddAuthentication` all'interno del metodo di `ConfigureServices` di *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Nel frammento di codice precedente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").

In alternativa, usare una versione di overload del metodo `AddAuthentication` per impostare più di una proprietà. Nell'esempio di metodo di overload seguente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme`. Lo schema di autenticazione può essere specificato in alternativa all'interno dei singoli attributi di `[Authorize]` o dei criteri di autorizzazione.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Definire uno schema predefinito in 2,0 se si verifica una delle condizioni seguenti:
- Si vuole che l'utente sia connesso automaticamente
- Usare l'attributo `[Authorize]` o i criteri di autorizzazione senza specificare gli schemi

Un'eccezione a questa regola è rappresentata dal metodo `AddIdentity`. Questo metodo aggiunge cookie per l'utente e imposta gli schemi di autenticazione e di verifica predefiniti per il cookie dell'applicazione `IdentityConstants.ApplicationScheme`. Inoltre, imposta lo schema di accesso predefinito per il cookie esterno `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Usare le estensioni di autenticazione HttpContext

L'interfaccia `IAuthenticationManager` è il punto di ingresso principale nel sistema di autenticazione 1. x. È stata sostituita con un nuovo set di `HttpContext` metodi di estensione nello spazio dei nomi `Microsoft.AspNetCore.Authentication`.

I progetti 1. x, ad esempio, fanno riferimento a una proprietà `Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Nei progetti 2,0 importare lo spazio dei nomi `Microsoft.AspNetCore.Authentication` ed eliminare i riferimenti alle proprietà `Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Autenticazione di Windows (HTTP. sys/IISIntegration)

Esistono due varianti di autenticazione di Windows:

* L'host consente solo gli utenti autenticati. Questa variante non è interessata dalle modifiche apportate al 2,0.
* L'host consente utenti anonimi e autenticati. Questa variazione è interessata dalle modifiche apportate a 2,0. Ad esempio, l'app deve consentire utenti anonimi a livello di [IIS](xref:host-and-deploy/iis/index) o [http. sys](xref:fundamentals/servers/httpsys) , ma autorizzare gli utenti a livello di controller. In questo scenario, impostare lo schema predefinito nel metodo `Startup.ConfigureServices`.

  Per [Microsoft. AspNetCore. Server. IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), impostare lo schema predefinito su `IISDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  Per [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), impostare lo schema predefinito su `HttpSysDefaults.AuthenticationScheme`:

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  Se non si imposta lo schema predefinito, la richiesta autorizza (Challenge) non funziona con l'eccezione seguente:

  > `System.InvalidOperationException`: non è stato specificato alcun authenticationScheme e non è stato trovato alcun DefaultChallengeScheme.

Per ulteriori informazioni, vedere <xref:security/authentication/windowsauth>.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Istanze di IdentityCookieOptions

Un effetto collaterale delle modifiche 2,0 è il passaggio all'uso delle opzioni denominate anziché delle istanze di opzioni dei cookie. La possibilità di personalizzare i nomi degli schemi dei cookie di identità viene rimossa.

Ad esempio, i progetti 1. x usano il [Costruttore Injection](xref:mvc/controllers/dependency-injection#constructor-injection) per passare un parametro di `IdentityCookieOptions` in *AccountController.cs* e *ManageController.cs*. È possibile accedere allo schema di autenticazione del cookie esterno dall'istanza di specificata:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

L'inserimento del costruttore precedente diventa superfluo nei progetti 2,0 e il campo `_externalCookieScheme` può essere eliminato:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

1. x i progetti usavano il campo `_externalCookieScheme` come indicato di seguito:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Nei progetti 2,0 sostituire il codice precedente con quello riportato di seguito. La costante `IdentityConstants.ExternalScheme` può essere utilizzata direttamente.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Risolvere la chiamata `SignOutAsync` appena aggiunta importando lo spazio dei nomi seguente:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Aggiungere le proprietà di navigazione POCO IdentityUser

Le proprietà di navigazione principale Entity Framework (EF) dell'`IdentityUser` di base POCO (Plain Old CLR Object) sono state rimosse. Se nel progetto 1. x sono state usate queste proprietà, aggiungerle manualmente al progetto 2,0:

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

Per evitare chiavi esterne duplicate durante l'esecuzione di EF Core migrazioni, aggiungere il codice seguente al metodo `OnModelCreating` della classe `IdentityDbContext` (dopo la chiamata `base.OnModelCreating();`):

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

## <a name="replace-getexternalauthenticationschemes"></a>Sostituisci GetExternalAuthenticationSchemes

Il metodo sincrono `GetExternalAuthenticationSchemes` è stato rimosso a favore di una versione asincrona. i progetti 1. x hanno il seguente codice in *Controllers/ManageController. cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Questo metodo viene visualizzato in *views/account/login. cshtml* :

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

Nei progetti 2,0 usare il metodo <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>. La modifica in *ManageController.cs* è simile al codice seguente:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

In *login. cshtml*la proprietà `AuthenticationScheme` a cui si accede nel ciclo `foreach` diventa `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Modifica della proprietà ManageLoginsViewModel

Un oggetto `ManageLoginsViewModel` viene utilizzato nell'azione `ManageLogins` di *ManageController.cs*. Nei progetti di 1. x, il tipo restituito della proprietà `OtherLogins` dell'oggetto è `IList<AuthenticationDescription>`. Questo tipo restituito richiede l'importazione di `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Nei progetti 2,0, il tipo restituito viene modificato in `IList<AuthenticationScheme>`. Questo nuovo tipo restituito richiede la sostituzione dell'importazione `Microsoft.AspNetCore.Http.Authentication` con un'importazione `Microsoft.AspNetCore.Authentication`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

Per ulteriori informazioni, vedere la [discussione relativa al problema Auth 2,0](https://github.com/aspnet/Security/issues/1338) su GitHub.
