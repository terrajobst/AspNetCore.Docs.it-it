---
title: Eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0
author: scottaddie
description: Questo articolo illustra i passaggi più comuni per la migrazione di ASP.NET Core 1.x autenticazione e identità ad ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 086deac51af186012315d5b6a1236c92c8980037
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039247"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="42d4c-103">Eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="42d4c-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="42d4c-104">Dal [Scott Addie](https://github.com/scottaddie) e [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="42d4c-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="42d4c-105">ASP.NET Core 2.0 include un nuovo modello per l'autenticazione e [identità](xref:security/authentication/identity) che semplifica la configurazione mediante i servizi.</span><span class="sxs-lookup"><span data-stu-id="42d4c-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="42d4c-106">Applicazioni ASP.NET Core 1.x che usano l'autenticazione o identità possono essere aggiornate per usare il nuovo modello, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="42d4c-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="42d4c-107">Servizi e Middleware di autenticazione</span><span class="sxs-lookup"><span data-stu-id="42d4c-107">Authentication Middleware and services</span></span>

<span data-ttu-id="42d4c-108">Nei progetti di 1.x, viene configurata l'autenticazione tramite middleware.</span><span class="sxs-lookup"><span data-stu-id="42d4c-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="42d4c-109">Per ogni schema di autenticazione che si vuole supportare, viene richiamato un metodo di middleware.</span><span class="sxs-lookup"><span data-stu-id="42d4c-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="42d4c-110">Nell'esempio seguente 1.x consente di configurare l'autenticazione di Facebook con l'identità in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="42d4c-111">Nei 2.0 progetti, l'autenticazione viene configurata tramite i servizi.</span><span class="sxs-lookup"><span data-stu-id="42d4c-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="42d4c-112">Ogni schema di autenticazione è registrato nel `ConfigureServices` metodo di *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="42d4c-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="42d4c-113">Il `UseIdentity` metodo viene sostituito con `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="42d4c-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="42d4c-114">Nell'esempio seguente 2.0 consente di configurare l'autenticazione di Facebook con l'identità in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="42d4c-115">Il `UseAuthentication` metodo aggiunge un componente middleware di autenticazione single che è responsabile per l'autenticazione automatica e la gestione delle richieste di autenticazione remota.</span><span class="sxs-lookup"><span data-stu-id="42d4c-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="42d4c-116">Sostituisce tutti i componenti middleware singole con un componente del middleware comune.</span><span class="sxs-lookup"><span data-stu-id="42d4c-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="42d4c-117">Di seguito sono 2.0 istruzioni relative alla migrazione per ogni schema di autenticazione principale.</span><span class="sxs-lookup"><span data-stu-id="42d4c-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="42d4c-118">Autenticazione basata su cookie</span><span class="sxs-lookup"><span data-stu-id="42d4c-118">Cookie-based authentication</span></span>

<span data-ttu-id="42d4c-119">Selezionare una delle due opzioni seguenti e apportare le modifiche necessarie nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="42d4c-120">Utilizzo dei cookie con Identity</span><span class="sxs-lookup"><span data-stu-id="42d4c-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="42d4c-121">Sostituire `UseIdentity` con `UseAuthentication` nel `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="42d4c-122">Richiama il `AddIdentity` metodo nella `ConfigureServices` metodo per aggiungere i servizi di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="42d4c-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="42d4c-123">Facoltativamente, richiamare il `ConfigureApplicationCookie` o `ConfigureExternalCookie` metodo nella `ConfigureServices` metodo per modificare le impostazioni di cookie di identità.</span><span class="sxs-lookup"><span data-stu-id="42d4c-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="42d4c-124">Uso dei cookie senza Identity</span><span class="sxs-lookup"><span data-stu-id="42d4c-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="42d4c-125">Sostituire il `UseCookieAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="42d4c-126">Richiama il `AddAuthentication` e `AddCookie` metodi nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="42d4c-127">Autenticazione della connessione JWT</span><span class="sxs-lookup"><span data-stu-id="42d4c-127">JWT Bearer Authentication</span></span>

<span data-ttu-id="42d4c-128">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="42d4c-129">Sostituire il `UseJwtBearerAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="42d4c-130">Richiama il `AddJwtBearer` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="42d4c-131">Questo frammento di codice non Usa identità, in modo che lo schema predefinito deve essere impostato passando `JwtBearerDefaults.AuthenticationScheme` per il `AddAuthentication` (metodo).</span><span class="sxs-lookup"><span data-stu-id="42d4c-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="42d4c-132">Autenticazione di OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="42d4c-132">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="42d4c-133">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="42d4c-134">Sostituire il `UseOpenIdConnectAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="42d4c-135">Richiama il `AddOpenIdConnect` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="42d4c-136">Sostituire il `PostLogoutRedirectUri` proprietà di `OpenIdConnectOptions` azione con `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-136">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="42d4c-137">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="42d4c-137">Facebook authentication</span></span>

<span data-ttu-id="42d4c-138">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="42d4c-139">Sostituire il `UseFacebookAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="42d4c-140">Richiama il `AddFacebook` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="42d4c-141">Autenticazione Google</span><span class="sxs-lookup"><span data-stu-id="42d4c-141">Google authentication</span></span>

<span data-ttu-id="42d4c-142">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="42d4c-143">Sostituire il `UseGoogleAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="42d4c-144">Richiama il `AddGoogle` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="42d4c-145">Autenticazione dell'Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="42d4c-145">Microsoft Account authentication</span></span>

<span data-ttu-id="42d4c-146">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="42d4c-147">Sostituire il `UseMicrosoftAccountAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="42d4c-148">Richiama il `AddMicrosoftAccount` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="42d4c-149">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="42d4c-149">Twitter authentication</span></span>

<span data-ttu-id="42d4c-150">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="42d4c-151">Sostituire il `UseTwitterAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="42d4c-152">Richiama il `AddTwitter` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="42d4c-153">Schemi di autenticazione predefinito di impostazione</span><span class="sxs-lookup"><span data-stu-id="42d4c-153">Setting default authentication schemes</span></span>

<span data-ttu-id="42d4c-154">Nella versione 1.x, la `AutomaticAuthenticate` e `AutomaticChallenge` delle proprietà delle [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe di base sono state deve essere impostata su uno schema di autenticazione single.</span><span class="sxs-lookup"><span data-stu-id="42d4c-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="42d4c-155">Si è verificato alcun efficace per applicare questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="42d4c-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="42d4c-156">2.0, queste due proprietà sono stati rimossi come proprietà nelle singole `AuthenticationOptions` istanza.</span><span class="sxs-lookup"><span data-stu-id="42d4c-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="42d4c-157">Può essere configurati nel `AddAuthentication` chiamata al metodo all'interno di `ConfigureServices` metodo di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="42d4c-158">Nel frammento di codice precedente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").</span><span class="sxs-lookup"><span data-stu-id="42d4c-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="42d4c-159">In alternativa, usare una versione di overload di `AddAuthentication` metodo per impostare più di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="42d4c-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="42d4c-160">Nell'esempio seguente il metodo di overload, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="42d4c-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="42d4c-161">Lo schema di autenticazione in alternativa può essere specificato all'interno di singoli `[Authorize]` attributi o i criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="42d4c-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="42d4c-162">Definire uno schema predefinito in 2.0 se viene soddisfatta una delle condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="42d4c-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="42d4c-163">Si desidera che l'utente di essere connessi automaticamente</span><span class="sxs-lookup"><span data-stu-id="42d4c-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="42d4c-164">Si utilizza il `[Authorize]` i criteri di autorizzazione o di attributo senza specificare gli schemi</span><span class="sxs-lookup"><span data-stu-id="42d4c-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="42d4c-165">Un'eccezione a questa regola è la `AddIdentity` (metodo).</span><span class="sxs-lookup"><span data-stu-id="42d4c-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="42d4c-166">Questo metodo aggiunge i cookie per l'utente e imposta l'impostazione predefinita l'autenticazione e stimolare gli schemi per il cookie dell'applicazione `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="42d4c-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="42d4c-167">Imposta inoltre lo schema di accesso predefinito per il cookie esterno `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="42d4c-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="42d4c-168">Usare le estensioni di autenticazione HttpContext</span><span class="sxs-lookup"><span data-stu-id="42d4c-168">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="42d4c-169">Il `IAuthenticationManager` interfaccia è il punto di ingresso principale nel sistema di autenticazione 1.x.</span><span class="sxs-lookup"><span data-stu-id="42d4c-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="42d4c-170">È stato sostituito con un nuovo set di `HttpContext` metodi di estensione di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="42d4c-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="42d4c-171">Progetti di riferimento, ad esempio, 1.x un `Authentication` proprietà:</span><span class="sxs-lookup"><span data-stu-id="42d4c-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="42d4c-172">Nei 2.0 progetti, importare il `Microsoft.AspNetCore.Authentication` dello spazio dei nomi ed eliminare il `Authentication` riferimenti di proprietà:</span><span class="sxs-lookup"><span data-stu-id="42d4c-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="42d4c-173">L'autenticazione di Windows (http. sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="42d4c-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="42d4c-174">Esistono due varianti di autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="42d4c-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="42d4c-175">L'host consente solo agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="42d4c-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="42d4c-176">L'host consente sia anonimo e gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="42d4c-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="42d4c-177">Non è interessata dalle 2.0 modifiche prima variante descritta in precedenza.</span><span class="sxs-lookup"><span data-stu-id="42d4c-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="42d4c-178">La seconda variante descritta in precedenza è interessata dalle modifiche 2.0.</span><span class="sxs-lookup"><span data-stu-id="42d4c-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="42d4c-179">Ad esempio, si potrebbero essere che consente agli utenti anonimi nella tua app in IIS o [HTTP. sys](xref:fundamentals/servers/httpsys) utenti ma per l'autorizzazione a livello di Controller di livello.</span><span class="sxs-lookup"><span data-stu-id="42d4c-179">As an example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="42d4c-180">In questo scenario, impostare lo schema predefinito `IISDefaults.AuthenticationScheme` nella `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="42d4c-181">Impossibile impostare lo schema predefinito di conseguenza impedisce la richiesta di autorizzazione per fare in modo da in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="42d4c-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="42d4c-182">Istanze IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="42d4c-182">IdentityCookieOptions instances</span></span>

<span data-ttu-id="42d4c-183">Un effetto collaterale delle 2.0 modifiche è l'opzione per l'utilizzo denominate opzioni anziché le istanze delle opzioni cookie.</span><span class="sxs-lookup"><span data-stu-id="42d4c-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="42d4c-184">La possibilità di personalizzare i nomi di schema di cookie di identità viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="42d4c-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="42d4c-185">Progetti di 1.x, ad esempio, usare [inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per passare un `IdentityCookieOptions` parametri in *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="42d4c-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="42d4c-186">Lo schema di autenticazione esterni cookie è accessibile dall'istanza fornita:</span><span class="sxs-lookup"><span data-stu-id="42d4c-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="42d4c-187">L'inserimento del costruttore menzionati in precedenza non è più necessario nei 2.0 progetti e `_externalCookieScheme` campo può essere eliminato:</span><span class="sxs-lookup"><span data-stu-id="42d4c-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="42d4c-188">Il `IdentityConstants.ExternalScheme` costante può essere utilizzata direttamente:</span><span class="sxs-lookup"><span data-stu-id="42d4c-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="42d4c-189">Aggiungere le proprietà di navigazione IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="42d4c-189">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="42d4c-190">Le proprietà di navigazione di Entity Framework (EF) Core della base `IdentityUser` sono state rimosse POCO (Plain Old CLR Object).</span><span class="sxs-lookup"><span data-stu-id="42d4c-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="42d4c-191">Se il progetto di 1.x Usa queste proprietà, aggiungerli manualmente al progetto 2.0:</span><span class="sxs-lookup"><span data-stu-id="42d4c-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="42d4c-192">Per evitare duplicate chiavi esterne durante l'esecuzione di migrazioni EF Core, aggiungere il codice seguente per il `IdentityDbContext` della classe `OnModelCreating` metodo (dopo il `base.OnModelCreating();` chiamare):</span><span class="sxs-lookup"><span data-stu-id="42d4c-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="42d4c-193">Sostituire GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="42d4c-193">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="42d4c-194">Metodo sincrono `GetExternalAuthenticationSchemes` è stato rimosso a favore di una versione asincrona.</span><span class="sxs-lookup"><span data-stu-id="42d4c-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="42d4c-195">i progetti di 1.x avere il codice seguente *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="42d4c-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="42d4c-196">Questo metodo viene visualizzato nella *cshtml* troppo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="42d4c-197">Nei 2.0 progetti, usare il `GetExternalAuthenticationSchemesAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="42d4c-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="42d4c-198">In *cshtml*, il `AuthenticationScheme` proprietà a cui accede nel `foreach` ciclo diventa `Name`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="42d4c-199">Modifica della proprietà ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="42d4c-199">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="42d4c-200">Oggetto `ManageLoginsViewModel` oggetto viene utilizzato nel `ManageLogins` azione di *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="42d4c-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="42d4c-201">Nei progetti di 1.x, l'oggetto `OtherLogins` proprietà tipo restituito è `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="42d4c-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="42d4c-202">Il tipo restituito richiede un'importazione di `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="42d4c-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="42d4c-203">Nei 2.0 progetti, il tipo restituito viene impostato su `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="42d4c-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="42d4c-204">Questo nuovo tipo restituito è necessario sostituire il `Microsoft.AspNetCore.Http.Authentication` importare con un `Microsoft.AspNetCore.Authentication` importare.</span><span class="sxs-lookup"><span data-stu-id="42d4c-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="42d4c-205">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="42d4c-205">Additional resources</span></span>

<span data-ttu-id="42d4c-206">Per ulteriori informazioni e discussione, vedere la [discussione per 2.0 Auth](https://github.com/aspnet/Security/issues/1338) problema in GitHub.</span><span class="sxs-lookup"><span data-stu-id="42d4c-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
