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
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="95b65-103">Autorizzare con uno schema specifico in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95b65-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="95b65-104">IIn alcuni scenari, come applicazioni a pagina singola (SPAs), è frequente l'utilizzo di più metodi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95b65-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="95b65-105">Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="95b65-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="95b65-106">In alcuni casi, l'app può avere più istanze di un gestore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95b65-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="95b65-107">Ad esempio, due gestori di cookie in cui uno contenente un'identità di base e uno viene creato quando è stata attivata l'autenticazione a più fattori (MFA).</span><span class="sxs-lookup"><span data-stu-id="95b65-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="95b65-108">Autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede sicurezza aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="95b65-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95b65-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95b65-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="95b65-110">Uno schema di autenticazione denominato quando il servizio di autenticazione viene configurato durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95b65-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="95b65-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95b65-111">For example:</span></span>

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

<span data-ttu-id="95b65-112">Nel codice precedente, sono state aggiunte due gestori di autenticazione: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="95b65-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="95b65-113">Specificando lo schema predefinito viene generato il `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="95b65-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="95b65-114">Se non è necessario tale comportamento, disattivarlo richiamando la forma senza parametri di `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="95b65-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95b65-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95b65-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95b65-116">Gli schemi di autenticazione sono denominati quando il middleware di autenticazione vengono configurato durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95b65-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="95b65-117">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95b65-117">For example:</span></span>

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

<span data-ttu-id="95b65-118">Nel codice precedente, sono state aggiunte due middleware di autenticazione: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="95b65-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="95b65-119">Specificando lo schema predefinito viene generato il `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="95b65-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="95b65-120">Se tale comportamento non è necessario, disabilitarla impostando il `AuthenticationOptions.AutomaticAuthenticate` proprietà `false`.</span><span class="sxs-lookup"><span data-stu-id="95b65-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="95b65-121">Selezionare lo schema con l'attributo Authorize</span><span class="sxs-lookup"><span data-stu-id="95b65-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="95b65-122">Al momento di autorizzazione, l'app indica il gestore da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="95b65-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="95b65-123">Selezionare il gestore con cui l'app verrà autorizzare passando un elenco delimitato da virgole di schemi di autenticazione `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="95b65-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="95b65-124">Il `[Authorize]` attributo specifica lo schema di autenticazione o gli schemi da usare indipendentemente dal fatto che sia configurato un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="95b65-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="95b65-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95b65-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95b65-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95b65-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95b65-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95b65-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="95b65-128">Nell'esempio precedente, gestori bearer sia il cookie eseguiti e hanno la possibilità di creare e aggiungere un'identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="95b65-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="95b65-129">Specificando un solo schema, viene eseguito il gestore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="95b65-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95b65-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95b65-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95b65-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95b65-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="95b65-132">Nel codice precedente, viene eseguito solo il gestore con lo schema "Bearer".</span><span class="sxs-lookup"><span data-stu-id="95b65-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="95b65-133">Qualsiasi identità basata su cookie vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="95b65-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="95b65-134">Selezionare lo schema con i criteri</span><span class="sxs-lookup"><span data-stu-id="95b65-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="95b65-135">Se si preferisce specificare le combinazioni di desiderato [criteri](xref:security/authorization/policies), è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio:</span><span class="sxs-lookup"><span data-stu-id="95b65-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="95b65-136">Nell'esempio precedente, il criterio "Over18" viene eseguito solo in base all'identità creato dal gestore "Bearer".</span><span class="sxs-lookup"><span data-stu-id="95b65-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="95b65-137">Usare i criteri impostando il `[Authorize]` dell'attributo `Policy` proprietà:</span><span class="sxs-lookup"><span data-stu-id="95b65-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="95b65-138">Usare più schemi di autenticazione</span><span class="sxs-lookup"><span data-stu-id="95b65-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="95b65-139">Alcune App potrebbe essere necessario supportare più tipi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95b65-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="95b65-140">Ad esempio, l'app può autenticare gli utenti da Azure Active Directory e da un database di utenti.</span><span class="sxs-lookup"><span data-stu-id="95b65-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="95b65-141">Un altro esempio è un'app che autentica gli utenti da Active Directory Federation Services e Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="95b65-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="95b65-142">In questo caso, l'app deve accettare un token di connessione JWT di autorità emittenti diversi.</span><span class="sxs-lookup"><span data-stu-id="95b65-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="95b65-143">Aggiungere tutti gli schemi di autenticazione che si vuole accettare.</span><span class="sxs-lookup"><span data-stu-id="95b65-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="95b65-144">Ad esempio, il codice seguente `Startup.ConfigureServices` aggiunge due schemi di autenticazione bearer token JWT con emittenti diversi:</span><span class="sxs-lookup"><span data-stu-id="95b65-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

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
> <span data-ttu-id="95b65-145">Un solo l'autenticazione bearer token JWT è registrata con lo schema di autenticazione predefinito `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="95b65-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="95b65-146">L'autenticazione aggiuntiva deve essere registrato con uno schema di autenticazione univoche.</span><span class="sxs-lookup"><span data-stu-id="95b65-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="95b65-147">Il passaggio successivo consiste nell'aggiornare i criteri di autorizzazione predefiniti per accettare entrambi gli schemi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="95b65-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="95b65-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="95b65-148">For example:</span></span>

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

<span data-ttu-id="95b65-149">I criteri di autorizzazione predefinito vengano eseguito l'override, è possibile usare un semplice `[Authorize]` attributo nel controller.</span><span class="sxs-lookup"><span data-stu-id="95b65-149">As the default authorization policy is overridden, it's possible to use a simple `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="95b65-150">Quindi, il controller accetta le richieste con token JWT rilasciato dall'autorità emittente primo o secondo.</span><span class="sxs-lookup"><span data-stu-id="95b65-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
