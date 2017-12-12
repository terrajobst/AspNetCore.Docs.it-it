---
title: Autorizzazione con uno specifico schema - ASP.NET Core
author: rick-anderson
description: "In questo articolo viene illustrato come limitare l'identità per uno schema specifico quando si lavora con più metodi di autenticazione."
keywords: "ASP.NET Core, identità, schema di autenticazione"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 8c9d068b88263d0c06b11a6b87416fb02885c475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="859c9-104">Autorizzazione con uno schema specifico</span><span class="sxs-lookup"><span data-stu-id="859c9-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="859c9-105">In alcuni scenari, ad esempio applicazioni a pagina singola (stabilimenti termali), è comune per utilizzare più metodi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="859c9-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="859c9-106">Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="859c9-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="859c9-107">In alcuni casi, l'app può avere più istanze di un gestore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="859c9-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="859c9-108">Ad esempio, due gestori di cookie in cui uno contiene un'identità di base e uno viene creato quando un'autenticazione a più fattori (MFA) è stata attivata.</span><span class="sxs-lookup"><span data-stu-id="859c9-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="859c9-109">Autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza.</span><span class="sxs-lookup"><span data-stu-id="859c9-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="859c9-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="859c9-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="859c9-111">Uno schema di autenticazione denominato quando il servizio di autenticazione viene configurato durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="859c9-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="859c9-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="859c9-112">For example:</span></span>

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

<span data-ttu-id="859c9-113">Nel codice precedente, sono stati aggiunti due gestori di autenticazione: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="859c9-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="859c9-114">Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="859c9-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="859c9-115">Se non si desidera questo comportamento, disattivarlo richiamando la forma senza parametri di `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="859c9-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="859c9-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="859c9-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="859c9-117">Gli schemi di autenticazione sono denominati quando l'autenticazione middlewares vengono configurati durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="859c9-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="859c9-118">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="859c9-118">For example:</span></span>

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

<span data-ttu-id="859c9-119">Nel codice precedente, sono stati aggiunti due autenticazione middlewares: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="859c9-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="859c9-120">Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="859c9-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="859c9-121">Questo comportamento non è possibile disabilitarlo impostando il `AuthenticationOptions.AutomaticAuthenticate` proprietà `false`.</span><span class="sxs-lookup"><span data-stu-id="859c9-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="859c9-122">Selezionare lo schema con l'attributo Authorize</span><span class="sxs-lookup"><span data-stu-id="859c9-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="859c9-123">Al momento di autorizzazione, l'app indica il gestore da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="859c9-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="859c9-124">Selezionare il gestore con cui l'app autorizzerà passando un elenco delimitato da virgole di schemi di autenticazione `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="859c9-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="859c9-125">Il `[Authorize]` attributo specifica lo schema di autenticazione o il schemi da utilizzare, indipendentemente dal fatto che sia stata configurata un'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="859c9-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="859c9-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="859c9-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="859c9-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="859c9-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="859c9-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="859c9-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="859c9-129">Nell'esempio precedente, i gestori di connessione sia il cookie eseguire e la possibilità di creare e aggiungere un'identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="859c9-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="859c9-130">Specificando un unico schema, viene eseguito il gestore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="859c9-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="859c9-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="859c9-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="859c9-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="859c9-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="859c9-133">Nel codice precedente, viene eseguito solo il gestore con lo schema "Bearer".</span><span class="sxs-lookup"><span data-stu-id="859c9-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="859c9-134">Vengono ignorate le identità basate su cookie.</span><span class="sxs-lookup"><span data-stu-id="859c9-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="859c9-135">Lo schema con i criteri di selezione</span><span class="sxs-lookup"><span data-stu-id="859c9-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="859c9-136">Se si preferisce specificare gli schemi desiderati in [criteri](xref:security/authorization/policies), è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio:</span><span class="sxs-lookup"><span data-stu-id="859c9-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="859c9-137">Nell'esempio precedente, il criterio "Over18" viene eseguito solo in base all'identità creata dal gestore "Bearer".</span><span class="sxs-lookup"><span data-stu-id="859c9-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="859c9-138">Usare i criteri impostando il `[Authorize]` dell'attributo `Policy` proprietà:</span><span class="sxs-lookup"><span data-stu-id="859c9-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
