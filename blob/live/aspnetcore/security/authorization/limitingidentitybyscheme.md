---
title: Autorizzazione con uno specifico schema - ASP.NET Core
author: rick-anderson
description: "In questo articolo viene illustrato come limitare l'identità per uno schema specifico quando si lavora con più metodi di autenticazione."
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 099dba1a4235ef62ea298748645b99e2d6d12d44
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="dc5ba-103">Autorizzazione con uno schema specifico</span><span class="sxs-lookup"><span data-stu-id="dc5ba-103">Authorize with a specific scheme</span></span>

<span data-ttu-id="dc5ba-104">In alcuni scenari, ad esempio applicazioni a pagina singola (stabilimenti termali), è comune per utilizzare più metodi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="dc5ba-105">Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="dc5ba-106">In alcuni casi, l'app può avere più istanze di un gestore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="dc5ba-107">Ad esempio, due gestori di cookie in cui uno contiene un'identità di base e uno viene creato quando un'autenticazione a più fattori (MFA) è stata attivata.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="dc5ba-108">Autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc5ba-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc5ba-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dc5ba-110">Uno schema di autenticazione denominato quando il servizio di autenticazione viene configurato durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="dc5ba-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dc5ba-111">For example:</span></span>

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

<span data-ttu-id="dc5ba-112">Nel codice precedente, sono stati aggiunti due gestori di autenticazione: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="dc5ba-113">Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="dc5ba-114">Se non si desidera questo comportamento, disattivarlo richiamando la forma senza parametri di `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc5ba-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc5ba-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dc5ba-116">Gli schemi di autenticazione sono denominati quando l'autenticazione middlewares vengono configurati durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="dc5ba-117">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dc5ba-117">For example:</span></span>

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

<span data-ttu-id="dc5ba-118">Nel codice precedente, sono stati aggiunti due autenticazione middlewares: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="dc5ba-119">Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="dc5ba-120">Questo comportamento non è possibile disabilitarlo impostando il `AuthenticationOptions.AutomaticAuthenticate` proprietà `false`.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="dc5ba-121">Selezionare lo schema con l'attributo Authorize</span><span class="sxs-lookup"><span data-stu-id="dc5ba-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="dc5ba-122">Al momento di autorizzazione, l'app indica il gestore da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="dc5ba-123">Selezionare il gestore con cui l'app autorizzerà passando un elenco delimitato da virgole di schemi di autenticazione `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="dc5ba-124">Il `[Authorize]` attributo specifica lo schema di autenticazione o il schemi da utilizzare, indipendentemente dal fatto che sia stata configurata un'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="dc5ba-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dc5ba-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc5ba-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc5ba-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc5ba-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc5ba-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="dc5ba-128">Nell'esempio precedente, i gestori di connessione sia il cookie eseguire e la possibilità di creare e aggiungere un'identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="dc5ba-129">Specificando un unico schema, viene eseguito il gestore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc5ba-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc5ba-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc5ba-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc5ba-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="dc5ba-132">Nel codice precedente, viene eseguito solo il gestore con lo schema "Bearer".</span><span class="sxs-lookup"><span data-stu-id="dc5ba-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="dc5ba-133">Vengono ignorate le identità basate su cookie.</span><span class="sxs-lookup"><span data-stu-id="dc5ba-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="dc5ba-134">Lo schema con i criteri di selezione</span><span class="sxs-lookup"><span data-stu-id="dc5ba-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="dc5ba-135">Se si preferisce specificare gli schemi desiderati in [criteri](xref:security/authorization/policies), è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio:</span><span class="sxs-lookup"><span data-stu-id="dc5ba-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="dc5ba-136">Nell'esempio precedente, il criterio "Over18" viene eseguito solo in base all'identità creata dal gestore "Bearer".</span><span class="sxs-lookup"><span data-stu-id="dc5ba-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="dc5ba-137">Usare i criteri impostando il `[Authorize]` dell'attributo `Policy` proprietà:</span><span class="sxs-lookup"><span data-stu-id="dc5ba-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
