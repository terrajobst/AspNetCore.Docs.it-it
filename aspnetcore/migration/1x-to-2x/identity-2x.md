---
title: "La migrazione di autenticazione e identità per i componenti di base di ASP.NET 2.0"
author: scottaddie
description: "Questo articolo vengono illustrati i passaggi più comuni per la migrazione ASP.NET Core 1. x autenticazione e identità per ASP.NET 2.0 Core."
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: dd48b2b027d22b570aa182e748ca91738e935f49
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="0fa12-103">La migrazione di autenticazione e identità per i componenti di base di ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="0fa12-103">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="0fa12-104">Da [Scott Addie](https://github.com/scottaddie) e [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="0fa12-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="0fa12-105">Componenti di base di ASP.NET 2.0 è un nuovo modello per l'autenticazione e [identità](xref:security/authentication/identity) che semplifica la configurazione mediante i servizi.</span><span class="sxs-lookup"><span data-stu-id="0fa12-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="0fa12-106">Applicazioni ASP.NET Core 1. x che utilizzano l'autenticazione o identità possono essere aggiornate per utilizzare il nuovo modello, come descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="0fa12-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="0fa12-107">Middleware di autenticazione e i servizi</span><span class="sxs-lookup"><span data-stu-id="0fa12-107">Authentication Middleware and services</span></span>
<span data-ttu-id="0fa12-108">In progetti di 1. x, è configurata l'autenticazione tramite il middleware.</span><span class="sxs-lookup"><span data-stu-id="0fa12-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="0fa12-109">Viene richiamato un metodo di middleware per ogni schema di autenticazione che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="0fa12-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="0fa12-110">Nell'esempio seguente di 1. x viene configurata l'autenticazione di Facebook con identità nella *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="0fa12-111">Nei progetti a 2.0, l'autenticazione viene configurata tramite i servizi.</span><span class="sxs-lookup"><span data-stu-id="0fa12-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="0fa12-112">Ogni schema di autenticazione è registrato nel `ConfigureServices` metodo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0fa12-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="0fa12-113">Il `UseIdentity` (metodo) viene sostituito con `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="0fa12-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="0fa12-114">Nell'esempio seguente viene 2.0 viene configurata l'autenticazione di Facebook con identità nella *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="0fa12-115">Il `UseAuthentication` metodo aggiunge un componente di middleware di autenticazione single che è responsabile per l'autenticazione automatica e la gestione delle richieste di autenticazione remota.</span><span class="sxs-lookup"><span data-stu-id="0fa12-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="0fa12-116">Sostituisce tutti i componenti middleware singoli con un componente del middleware comune.</span><span class="sxs-lookup"><span data-stu-id="0fa12-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="0fa12-117">Di seguito sono 2.0 istruzioni relative alla migrazione per ogni schema di autenticazione principale.</span><span class="sxs-lookup"><span data-stu-id="0fa12-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="0fa12-118">Autenticazione basata su cookie</span><span class="sxs-lookup"><span data-stu-id="0fa12-118">Cookie-based authentication</span></span>
<span data-ttu-id="0fa12-119">Selezionare una delle due opzioni seguenti e apportare le modifiche necessarie in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="0fa12-120">Utilizzare i cookie con identità</span><span class="sxs-lookup"><span data-stu-id="0fa12-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="0fa12-121">Sostituire `UseIdentity` con `UseAuthentication` nel `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="0fa12-122">Richiamare il `AddIdentity` metodo il `ConfigureServices` metodo per aggiungere i servizi di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="0fa12-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="0fa12-123">Facoltativamente, richiamare il `ConfigureApplicationCookie` o `ConfigureExternalCookie` metodo il `ConfigureServices` metodo per modificare le impostazioni dei cookie di identità.</span><span class="sxs-lookup"><span data-stu-id="0fa12-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="0fa12-124">Utilizzare i cookie senza identità</span><span class="sxs-lookup"><span data-stu-id="0fa12-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="0fa12-125">Sostituire il `UseCookieAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="0fa12-126">Richiamare il `AddAuthentication` e `AddCookie` metodi di `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="0fa12-127">Autenticazione della connessione JWT</span><span class="sxs-lookup"><span data-stu-id="0fa12-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="0fa12-128">Apportare le modifiche seguenti in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0fa12-129">Sostituire il `UseJwtBearerAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0fa12-130">Richiamare il `AddJwtBearer` metodo il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="0fa12-131">Questo frammento di codice non usa l'identità, pertanto lo schema predefinito deve essere impostato passando `JwtBearerDefaults.AuthenticationScheme` per il `AddAuthentication` metodo.</span><span class="sxs-lookup"><span data-stu-id="0fa12-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="0fa12-132">Autenticazione di OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="0fa12-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="0fa12-133">Apportare le modifiche seguenti in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="0fa12-134">Sostituire il `UseOpenIdConnectAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0fa12-135">Richiamare il `AddOpenIdConnect` metodo il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="0fa12-136">autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="0fa12-136">Facebook authentication</span></span>
<span data-ttu-id="0fa12-137">Apportare le modifiche seguenti in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0fa12-138">Sostituire il `UseFacebookAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0fa12-139">Richiamare il `AddFacebook` metodo il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="0fa12-140">Autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="0fa12-140">Google authentication</span></span>
<span data-ttu-id="0fa12-141">Apportare le modifiche seguenti in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0fa12-142">Sostituire il `UseGoogleAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0fa12-143">Richiamare il `AddGoogle` metodo il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="0fa12-144">Autenticazione dell'Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="0fa12-144">Microsoft Account authentication</span></span>
<span data-ttu-id="0fa12-145">Apportare le modifiche seguenti in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0fa12-146">Sostituire il `UseMicrosoftAccountAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0fa12-147">Richiamare il `AddMicrosoftAccount` metodo il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="0fa12-148">Autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="0fa12-148">Twitter authentication</span></span>
<span data-ttu-id="0fa12-149">Apportare le modifiche seguenti in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0fa12-150">Sostituire il `UseTwitterAuthentication` chiamata al metodo di `Configure` metodo con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0fa12-151">Richiamare il `AddTwitter` metodo il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="0fa12-152">L'impostazione di schemi di autenticazione predefinito</span><span class="sxs-lookup"><span data-stu-id="0fa12-152">Setting default authentication schemes</span></span>
<span data-ttu-id="0fa12-153">In 1. x, il `AutomaticAuthenticate` e `AutomaticChallenge` le proprietà del [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) sono stati deve essere impostata su uno schema di autenticazione singola classe di base.</span><span class="sxs-lookup"><span data-stu-id="0fa12-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="0fa12-154">Si è verificato non è possibile applicare questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="0fa12-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="0fa12-155">In 2.0 sono state rimosse queste due proprietà come proprietà per il singolo `AuthenticationOptions` istanza.</span><span class="sxs-lookup"><span data-stu-id="0fa12-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="0fa12-156">Può essere configurati nel `AddAuthentication` chiamata al metodo all'interno di `ConfigureServices` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="0fa12-157">Nel frammento di codice precedente, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").</span><span class="sxs-lookup"><span data-stu-id="0fa12-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="0fa12-158">In alternativa, utilizzare una versione di overload di `AddAuthentication` per impostare più di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="0fa12-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="0fa12-159">Nell'esempio seguente il metodo di overload, lo schema predefinito è impostato su `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0fa12-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="0fa12-160">In alternativa, lo schema di autenticazione può essere specificato all'interno di singoli `[Authorize]` attributi o i criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="0fa12-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="0fa12-161">Definire uno schema predefinito in 2.0 se viene soddisfatta una delle condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0fa12-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="0fa12-162">Si desidera che l'utente di essere connessi automaticamente</span><span class="sxs-lookup"><span data-stu-id="0fa12-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="0fa12-163">Utilizzare il `[Authorize]` i criteri di autorizzazione o di attributo senza specificare gli schemi</span><span class="sxs-lookup"><span data-stu-id="0fa12-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="0fa12-164">Un'eccezione a questa regola è la `AddIdentity` metodo.</span><span class="sxs-lookup"><span data-stu-id="0fa12-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="0fa12-165">Questo metodo aggiunge i cookie per l'utente e imposta il valore predefinito di autenticare e richiesta di schemi per il cookie dell'applicazione `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0fa12-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="0fa12-166">Inoltre, imposta lo schema predefinito di accesso nel cookie esterno `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="0fa12-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="0fa12-167">Utilizzare le estensioni di autenticazione HttpContext</span><span class="sxs-lookup"><span data-stu-id="0fa12-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="0fa12-168">Il `IAuthenticationManager` interfaccia è il punto di ingresso principale nel sistema di autenticazione di 1. x.</span><span class="sxs-lookup"><span data-stu-id="0fa12-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="0fa12-169">È stato sostituito con un nuovo set di `HttpContext` metodi di estensione di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0fa12-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="0fa12-170">Progetti di riferimento, ad esempio, 1. x un `Authentication` proprietà:</span><span class="sxs-lookup"><span data-stu-id="0fa12-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="0fa12-171">Nei progetti a 2.0, importare il `Microsoft.AspNetCore.Authentication` dello spazio dei nomi ed eliminare il `Authentication` riferimenti alle proprietà:</span><span class="sxs-lookup"><span data-stu-id="0fa12-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="0fa12-172">L'autenticazione di Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="0fa12-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="0fa12-173">Esistono due varianti dell'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="0fa12-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="0fa12-174">L'host consente solo agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="0fa12-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="0fa12-175">L'host consente sia anonimi e gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="0fa12-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="0fa12-176">La prima variante descritta in precedenza non è influenzata dalle 2.0 modifiche.</span><span class="sxs-lookup"><span data-stu-id="0fa12-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="0fa12-177">Descritto in precedenza mentre la seconda è interessata dalle 2.0 modifiche.</span><span class="sxs-lookup"><span data-stu-id="0fa12-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="0fa12-178">Ad esempio, si possono consentire agli utenti anonimi all'applicazione in IIS o [HTTP.sys](xref:fundamentals/servers/weblistener) livelli di autorizzazione ma gli utenti a livello di Controller.</span><span class="sxs-lookup"><span data-stu-id="0fa12-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="0fa12-179">In questo scenario, impostare lo schema predefinito `IISDefaults.AuthenticationScheme` nel `ConfigureServices` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="0fa12-180">Impossibile impostare lo schema predefinito impedisce conseguenza richiesta authorize di incentivare l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="0fa12-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="0fa12-181">Istanze di IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="0fa12-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="0fa12-182">Un effetto collaterale delle 2.0 modifiche è il passaggio a denominato opzioni anziché le istanze di opzioni di cookie.</span><span class="sxs-lookup"><span data-stu-id="0fa12-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="0fa12-183">La possibilità di personalizzare i nomi di schema di cookie di identità viene rimossa.</span><span class="sxs-lookup"><span data-stu-id="0fa12-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="0fa12-184">Ad esempio, 1. x progetti utilizzano [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per passare un `IdentityCookieOptions` parametro in *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0fa12-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="0fa12-185">Lo schema di autenticazione esterno cookie è accessibile dall'istanza fornita:</span><span class="sxs-lookup"><span data-stu-id="0fa12-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="0fa12-186">L'inserimento di un costruttore menzionati in precedenza non è necessaria nei progetti di 2.0 e `_externalCookieScheme` campo può essere eliminato:</span><span class="sxs-lookup"><span data-stu-id="0fa12-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="0fa12-187">Il `IdentityConstants.ExternalScheme` costante può essere utilizzata direttamente:</span><span class="sxs-lookup"><span data-stu-id="0fa12-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="0fa12-188">Aggiungere le proprietà di navigazione IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="0fa12-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="0fa12-189">Le proprietà di navigazione di base di Entity Framework (EF) della base `IdentityUser` sono state rimosse POCO (Plain precedente CLR Object).</span><span class="sxs-lookup"><span data-stu-id="0fa12-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="0fa12-190">Se il progetto di 1. x utilizza queste proprietà, aggiungerle manualmente al progetto 2.0:</span><span class="sxs-lookup"><span data-stu-id="0fa12-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="0fa12-191">Per evitare duplicate chiavi esterne durante l'esecuzione di migrazioni di Entity Framework Core, aggiungere il comando seguente per il `IdentityDbContext` classe `OnModelCreating` (metodo) (dopo il `base.OnModelCreating();` chiamare):</span><span class="sxs-lookup"><span data-stu-id="0fa12-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="0fa12-192">Sostituire GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="0fa12-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="0fa12-193">Il metodo sincrono `GetExternalAuthenticationSchemes` a favore di una versione asincrona è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="0fa12-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="0fa12-194">1. x progetti presentano nel codice seguente *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fa12-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="0fa12-195">Questo metodo viene visualizzato *cshtml* troppo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="0fa12-196">Nei 2.0 progetti, usare il `GetExternalAuthenticationSchemesAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="0fa12-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="0fa12-197">In *cshtml*, `AuthenticationScheme` proprietà accessibile nel `foreach` ciclo diventa `Name`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="0fa12-198">Modifica della proprietà ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="0fa12-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="0fa12-199">Oggetto `ManageLoginsViewModel` oggetto viene utilizzato nel `ManageLogins` azione di *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0fa12-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="0fa12-200">In del 1. x progetti, l'oggetto `OtherLogins` proprietà tipo restituito è `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="0fa12-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="0fa12-201">Il tipo restituito richiede un'importazione di `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="0fa12-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="0fa12-202">Nei progetti a 2.0, il tipo restituito viene modificato per `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="0fa12-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="0fa12-203">Questo nuovo tipo restituito è sostituire il `Microsoft.AspNetCore.Http.Authentication` importare con un `Microsoft.AspNetCore.Authentication` importare.</span><span class="sxs-lookup"><span data-stu-id="0fa12-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="0fa12-204">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0fa12-204">Additional resources</span></span>
<span data-ttu-id="0fa12-205">Per ulteriori dettagli e informazioni, vedere il [discussione per autenticazione 2.0](https://github.com/aspnet/Security/issues/1338) problema su GitHub.</span><span class="sxs-lookup"><span data-stu-id="0fa12-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
