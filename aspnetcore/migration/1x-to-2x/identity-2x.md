---
title: Eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0
author: scottaddie
description: Questo articolo illustra i passaggi più comuni per la migrazione di ASP.NET Core 1.x autenticazione e identità ad ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 10/26/2017
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 6d457d42ad29ca579ba74e3b097d143bd6531b72
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828983"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="f45fc-103">Eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="f45fc-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="f45fc-104">Dal [Scott Addie](https://github.com/scottaddie) e [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="f45fc-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="f45fc-105">ASP.NET Core 2.0 include un nuovo modello per l'autenticazione e [identità](xref:security/authentication/identity) che semplifica la configurazione mediante i servizi.</span><span class="sxs-lookup"><span data-stu-id="f45fc-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="f45fc-106">Applicazioni ASP.NET Core 1.x che usano l'autenticazione o identità possono essere aggiornate per usare il nuovo modello, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f45fc-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="f45fc-107">Servizi e Middleware di autenticazione</span><span class="sxs-lookup"><span data-stu-id="f45fc-107">Authentication Middleware and services</span></span>
<span data-ttu-id="f45fc-108">Nei progetti di 1.x, viene configurata l'autenticazione tramite middleware.</span><span class="sxs-lookup"><span data-stu-id="f45fc-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="f45fc-109">Per ogni schema di autenticazione che si vuole supportare, viene richiamato un metodo di middleware.</span><span class="sxs-lookup"><span data-stu-id="f45fc-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="f45fc-110">Nell'esempio seguente 1.x consente di configurare l'autenticazione di Facebook con l'identità in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="f45fc-111">Nei 2.0 progetti, l'autenticazione viene configurata tramite i servizi.</span><span class="sxs-lookup"><span data-stu-id="f45fc-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="f45fc-112">Ogni schema di autenticazione è registrato nel `ConfigureServices` metodo di *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f45fc-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="f45fc-113">Il `UseIdentity` metodo viene sostituito con `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="f45fc-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="f45fc-114">Nell'esempio seguente 2.0 consente di configurare l'autenticazione di Facebook con l'identità in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="f45fc-115">Il `UseAuthentication` metodo aggiunge un componente middleware di autenticazione single che è responsabile per l'autenticazione automatica e la gestione delle richieste di autenticazione remota.</span><span class="sxs-lookup"><span data-stu-id="f45fc-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="f45fc-116">Sostituisce tutti i componenti middleware singole con un componente del middleware comune.</span><span class="sxs-lookup"><span data-stu-id="f45fc-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="f45fc-117">Di seguito sono 2.0 istruzioni relative alla migrazione per ogni schema di autenticazione principale.</span><span class="sxs-lookup"><span data-stu-id="f45fc-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="f45fc-118">Autenticazione basata su cookie</span><span class="sxs-lookup"><span data-stu-id="f45fc-118">Cookie-based authentication</span></span>
<span data-ttu-id="f45fc-119">Selezionare una delle due opzioni seguenti e apportare le modifiche necessarie nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="f45fc-120">Utilizzo dei cookie con Identity</span><span class="sxs-lookup"><span data-stu-id="f45fc-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="f45fc-121">Sostituire `UseIdentity` con `UseAuthentication` nel `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="f45fc-122">Richiama il `AddIdentity` metodo nella `ConfigureServices` metodo per aggiungere i servizi di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fc-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="f45fc-123">Facoltativamente, richiamare il `ConfigureApplicationCookie` o `ConfigureExternalCookie` metodo nella `ConfigureServices` metodo per modificare le impostazioni di cookie di identità.</span><span class="sxs-lookup"><span data-stu-id="f45fc-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="f45fc-124">Uso dei cookie senza Identity</span><span class="sxs-lookup"><span data-stu-id="f45fc-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="f45fc-125">Sostituire il `UseCookieAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="f45fc-126">Richiama il `AddAuthentication` e `AddCookie` metodi nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="f45fc-127">Autenticazione della connessione JWT</span><span class="sxs-lookup"><span data-stu-id="f45fc-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="f45fc-128">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="f45fc-129">Sostituire il `UseJwtBearerAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="f45fc-130">Richiama il `AddJwtBearer` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="f45fc-131">Questo frammento di codice non Usa identità, in modo che lo schema predefinito deve essere impostato passando `JwtBearerDefaults.AuthenticationScheme` per il `AddAuthentication` (metodo).</span><span class="sxs-lookup"><span data-stu-id="f45fc-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="f45fc-132">Autenticazione di OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="f45fc-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="f45fc-133">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="f45fc-134">Sostituire il `UseOpenIdConnectAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="f45fc-135">Richiama il `AddOpenIdConnect` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="f45fc-136">autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="f45fc-136">Facebook authentication</span></span>
<span data-ttu-id="f45fc-137">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="f45fc-138">Sostituire il `UseFacebookAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="f45fc-139">Richiama il `AddFacebook` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="f45fc-140">Autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="f45fc-140">Google authentication</span></span>
<span data-ttu-id="f45fc-141">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="f45fc-142">Sostituire il `UseGoogleAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="f45fc-143">Richiama il `AddGoogle` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="f45fc-144">Autenticazione dell'Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="f45fc-144">Microsoft Account authentication</span></span>
<span data-ttu-id="f45fc-145">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="f45fc-146">Sostituire il `UseMicrosoftAccountAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="f45fc-147">Richiama il `AddMicrosoftAccount` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="f45fc-148">Autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="f45fc-148">Twitter authentication</span></span>
<span data-ttu-id="f45fc-149">Apportare le modifiche seguenti nel *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="f45fc-150">Sostituire il `UseTwitterAuthentication` chiamata al metodo il `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="f45fc-151">Richiama il `AddTwitter` metodo nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="f45fc-152">Schemi di autenticazione predefinito di impostazione</span><span class="sxs-lookup"><span data-stu-id="f45fc-152">Setting default authentication schemes</span></span>
<span data-ttu-id="f45fc-153">Nella versione 1.x, la `AutomaticAuthenticate` e `AutomaticChallenge` delle proprietà delle [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe di base sono state deve essere impostata su uno schema di autenticazione single.</span><span class="sxs-lookup"><span data-stu-id="f45fc-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="f45fc-154">Si è verificato alcun efficace per applicare questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="f45fc-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="f45fc-155">2.0, queste due proprietà sono stati rimossi come proprietà nelle singole `AuthenticationOptions` istanza.</span><span class="sxs-lookup"><span data-stu-id="f45fc-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="f45fc-156">Può essere configurati nel `AddAuthentication` chiamata al metodo all'interno di `ConfigureServices` metodo di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="f45fc-157">Nel frammento di codice precedente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").</span><span class="sxs-lookup"><span data-stu-id="f45fc-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="f45fc-158">In alternativa, usare una versione di overload di `AddAuthentication` metodo per impostare più di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="f45fc-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="f45fc-159">Nell'esempio seguente il metodo di overload, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="f45fc-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="f45fc-160">Lo schema di autenticazione in alternativa può essere specificato all'interno di singoli `[Authorize]` attributi o i criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f45fc-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="f45fc-161">Definire uno schema predefinito in 2.0 se viene soddisfatta una delle condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f45fc-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="f45fc-162">Si desidera che l'utente di essere connessi automaticamente</span><span class="sxs-lookup"><span data-stu-id="f45fc-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="f45fc-163">Si utilizza il `[Authorize]` i criteri di autorizzazione o di attributo senza specificare gli schemi</span><span class="sxs-lookup"><span data-stu-id="f45fc-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="f45fc-164">Un'eccezione a questa regola è la `AddIdentity` (metodo).</span><span class="sxs-lookup"><span data-stu-id="f45fc-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="f45fc-165">Questo metodo aggiunge i cookie per l'utente e imposta l'impostazione predefinita l'autenticazione e stimolare gli schemi per il cookie dell'applicazione `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="f45fc-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="f45fc-166">Imposta inoltre lo schema di accesso predefinito per il cookie esterno `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="f45fc-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="f45fc-167">Usare le estensioni di autenticazione HttpContext</span><span class="sxs-lookup"><span data-stu-id="f45fc-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="f45fc-168">Il `IAuthenticationManager` interfaccia è il punto di ingresso principale nel sistema di autenticazione 1.x.</span><span class="sxs-lookup"><span data-stu-id="f45fc-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="f45fc-169">È stato sostituito con un nuovo set di `HttpContext` metodi di estensione di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="f45fc-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="f45fc-170">Progetti di riferimento, ad esempio, 1.x un `Authentication` proprietà:</span><span class="sxs-lookup"><span data-stu-id="f45fc-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="f45fc-171">Nei 2.0 progetti, importare il `Microsoft.AspNetCore.Authentication` dello spazio dei nomi ed eliminare il `Authentication` riferimenti di proprietà:</span><span class="sxs-lookup"><span data-stu-id="f45fc-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="f45fc-172">L'autenticazione di Windows (http. sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="f45fc-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="f45fc-173">Esistono due varianti di autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="f45fc-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="f45fc-174">L'host consente solo agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="f45fc-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="f45fc-175">L'host consente sia anonimo e gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="f45fc-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="f45fc-176">Non è interessata dalle 2.0 modifiche prima variante descritta in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f45fc-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="f45fc-177">La seconda variante descritta in precedenza è interessata dalle modifiche 2.0.</span><span class="sxs-lookup"><span data-stu-id="f45fc-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="f45fc-178">Ad esempio, si potrebbero essere che consente agli utenti anonimi all'applicazione in IIS o [HTTP. sys](xref:fundamentals/servers/weblistener) utenti ma per l'autorizzazione a livello di Controller di livello.</span><span class="sxs-lookup"><span data-stu-id="f45fc-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="f45fc-179">In questo scenario, impostare lo schema predefinito `IISDefaults.AuthenticationScheme` nella `ConfigureServices` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="f45fc-180">Impossibile impostare lo schema predefinito di conseguenza impedisce la richiesta di autorizzazione per fare in modo da in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f45fc-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="f45fc-181">Istanze IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="f45fc-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="f45fc-182">Un effetto collaterale delle 2.0 modifiche è l'opzione per l'utilizzo denominate opzioni anziché le istanze delle opzioni cookie.</span><span class="sxs-lookup"><span data-stu-id="f45fc-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="f45fc-183">La possibilità di personalizzare i nomi di schema di cookie di identità viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="f45fc-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="f45fc-184">Progetti di 1.x, ad esempio, usare [inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per passare un `IdentityCookieOptions` parametri in *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="f45fc-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="f45fc-185">Lo schema di autenticazione esterni cookie è accessibile dall'istanza fornita:</span><span class="sxs-lookup"><span data-stu-id="f45fc-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="f45fc-186">L'inserimento del costruttore menzionati in precedenza non è più necessario nei 2.0 progetti e `_externalCookieScheme` campo può essere eliminato:</span><span class="sxs-lookup"><span data-stu-id="f45fc-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="f45fc-187">Il `IdentityConstants.ExternalScheme` costante può essere utilizzata direttamente:</span><span class="sxs-lookup"><span data-stu-id="f45fc-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="f45fc-188">Aggiungere le proprietà di navigazione IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="f45fc-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="f45fc-189">Le proprietà di navigazione di Entity Framework (EF) Core della base `IdentityUser` sono state rimosse POCO (Plain Old CLR Object).</span><span class="sxs-lookup"><span data-stu-id="f45fc-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="f45fc-190">Se il progetto di 1.x Usa queste proprietà, aggiungerli manualmente al progetto 2.0:</span><span class="sxs-lookup"><span data-stu-id="f45fc-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="f45fc-191">Per evitare duplicate chiavi esterne durante l'esecuzione di migrazioni EF Core, aggiungere il codice seguente per il `IdentityDbContext` della classe `OnModelCreating` metodo (dopo il `base.OnModelCreating();` chiamare):</span><span class="sxs-lookup"><span data-stu-id="f45fc-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="f45fc-192">Sostituire GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="f45fc-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="f45fc-193">Metodo sincrono `GetExternalAuthenticationSchemes` è stato rimosso a favore di una versione asincrona.</span><span class="sxs-lookup"><span data-stu-id="f45fc-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="f45fc-194">i progetti di 1.x avere il codice seguente *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="f45fc-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="f45fc-195">Questo metodo viene visualizzato nella *cshtml* troppo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="f45fc-196">Nei 2.0 progetti, usare il `GetExternalAuthenticationSchemesAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="f45fc-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="f45fc-197">In *cshtml*, il `AuthenticationScheme` proprietà a cui accede nel `foreach` ciclo diventa `Name`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="f45fc-198">Modifica della proprietà ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="f45fc-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="f45fc-199">Oggetto `ManageLoginsViewModel` oggetto viene utilizzato nel `ManageLogins` azione di *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="f45fc-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="f45fc-200">Nei progetti di 1.x, l'oggetto `OtherLogins` proprietà tipo restituito è `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="f45fc-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="f45fc-201">Il tipo restituito richiede un'importazione di `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="f45fc-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="f45fc-202">Nei 2.0 progetti, il tipo restituito viene impostato su `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="f45fc-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="f45fc-203">Questo nuovo tipo restituito è necessario sostituire il `Microsoft.AspNetCore.Http.Authentication` importare con un `Microsoft.AspNetCore.Authentication` importare.</span><span class="sxs-lookup"><span data-stu-id="f45fc-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="f45fc-204">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f45fc-204">Additional resources</span></span>
<span data-ttu-id="f45fc-205">Per ulteriori informazioni e discussione, vedere la [discussione per 2.0 Auth](https://github.com/aspnet/Security/issues/1338) problema in GitHub.</span><span class="sxs-lookup"><span data-stu-id="f45fc-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
