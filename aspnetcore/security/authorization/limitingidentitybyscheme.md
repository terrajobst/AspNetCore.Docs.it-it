---
title: Autorizzazione con uno schema specifico in ASP.NET Core
author: rick-anderson
description: In questo articolo viene illustrato come limitare l'identità per uno schema specifico quando si lavora con più metodi di autenticazione.
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 81a01d7de8221fcb3bf90a108d9df6633ca2b696
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072698"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="2809e-103">Autorizzazione con uno schema specifico in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2809e-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="2809e-104">IIn alcuni scenari, come applicazioni a pagina singola (SPAs), è frequente l'utilizzo di più metodi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2809e-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="2809e-105">Ad esempio, l'app può utilizzare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione JWT per le richieste di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2809e-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="2809e-106">In alcuni casi, l'app può avere più istanze di un gestore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2809e-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="2809e-107">Ad esempio, due gestori di cookie in cui uno contiene un'identità di base e uno viene creato quando un'autenticazione a più fattori (MFA) è stata attivata.</span><span class="sxs-lookup"><span data-stu-id="2809e-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="2809e-108">Autenticazione a più fattori può essere attivata perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza.</span><span class="sxs-lookup"><span data-stu-id="2809e-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2809e-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2809e-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2809e-110">Uno schema di autenticazione denominato quando il servizio di autenticazione viene configurato durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2809e-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="2809e-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2809e-111">For example:</span></span>

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

<span data-ttu-id="2809e-112">Nel codice precedente, sono stati aggiunti due gestori di autenticazione: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="2809e-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="2809e-113">Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="2809e-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="2809e-114">Se non si desidera questo comportamento, disattivarlo richiamando la forma senza parametri di `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2809e-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2809e-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2809e-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2809e-116">Gli schemi di autenticazione sono denominati quando l'autenticazione middlewares vengono configurati durante l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2809e-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="2809e-117">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2809e-117">For example:</span></span>

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

<span data-ttu-id="2809e-118">Nel codice precedente, sono stati aggiunti due autenticazione middlewares: uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="2809e-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="2809e-119">Specifica lo schema predefinito comporta la `HttpContext.User` proprietà viene impostata su tale identità.</span><span class="sxs-lookup"><span data-stu-id="2809e-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="2809e-120">Questo comportamento non è possibile disabilitarlo impostando il `AuthenticationOptions.AutomaticAuthenticate` proprietà `false`.</span><span class="sxs-lookup"><span data-stu-id="2809e-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="2809e-121">Selezionare lo schema con l'attributo Authorize</span><span class="sxs-lookup"><span data-stu-id="2809e-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="2809e-122">Al momento di autorizzazione, l'app indica il gestore da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="2809e-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="2809e-123">Selezionare il gestore con cui l'app autorizzerà passando un elenco delimitato da virgole di schemi di autenticazione `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="2809e-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="2809e-124">Il `[Authorize]` attributo specifica lo schema di autenticazione o il schemi da utilizzare, indipendentemente dal fatto che sia stata configurata un'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2809e-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="2809e-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2809e-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2809e-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2809e-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2809e-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2809e-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="2809e-128">Nell'esempio precedente, i gestori di connessione sia il cookie eseguire e la possibilità di creare e aggiungere un'identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="2809e-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="2809e-129">Specificando un unico schema, viene eseguito il gestore corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2809e-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2809e-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2809e-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2809e-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2809e-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="2809e-132">Nel codice precedente, viene eseguito solo il gestore con lo schema "Bearer".</span><span class="sxs-lookup"><span data-stu-id="2809e-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="2809e-133">Vengono ignorate le identità basate su cookie.</span><span class="sxs-lookup"><span data-stu-id="2809e-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="2809e-134">Lo schema con i criteri di selezione</span><span class="sxs-lookup"><span data-stu-id="2809e-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="2809e-135">Se si preferisce specificare gli schemi desiderati in [criteri](xref:security/authorization/policies), è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio:</span><span class="sxs-lookup"><span data-stu-id="2809e-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="2809e-136">Nell'esempio precedente, il criterio "Over18" viene eseguito solo in base all'identità creata dal gestore "Bearer".</span><span class="sxs-lookup"><span data-stu-id="2809e-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="2809e-137">Usare i criteri impostando il `[Authorize]` dell'attributo `Policy` proprietà:</span><span class="sxs-lookup"><span data-stu-id="2809e-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
