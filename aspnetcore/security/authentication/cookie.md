---
title: Usare l'autenticazione tramite cookie senza ASP.NET Core Identity
author: rick-anderson
description: Informazioni su come usare l'autenticazione tramite cookie senza ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622740"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="708f0-103">Usare l'autenticazione tramite cookie senza ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="708f0-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="708f0-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="708f0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="708f0-105">ASP.NET Core Identity è un provider di autenticazione completa e completo per creare e gestire gli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="708f0-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="708f0-106">Tuttavia, un provider di autenticazione l'autenticazione basata su cookie senza ASP.NET Core Identity può essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="708f0-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="708f0-107">Per altre informazioni, vedere <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="708f0-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="708f0-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="708f0-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="708f0-109">A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="708f0-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="708f0-110">Usare la **messaggio di posta elettronica** nome utente `maria.rodriguez@contoso.com` e qualsiasi password di accesso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="708f0-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="708f0-111">L'utente viene autenticato nel `AuthenticateUser` metodo nella *Pages/Account/Login.cshtml.cs* file.</span><span class="sxs-lookup"><span data-stu-id="708f0-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="708f0-112">In un esempio reale, l'utente viene autenticato in un database.</span><span class="sxs-lookup"><span data-stu-id="708f0-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="708f0-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="708f0-113">Configuration</span></span>

<span data-ttu-id="708f0-114">Se l'app non usa la [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="708f0-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="708f0-115">Nel `Startup.ConfigureServices` metodo, creare il servizio di Middleware di autenticazione con il <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> metodi:</span><span class="sxs-lookup"><span data-stu-id="708f0-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="708f0-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="708f0-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="708f0-117">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="708f0-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="708f0-118">Impostando il `AuthenticationScheme` al [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) fornisce un valore di "Cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="708f0-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="708f0-119">È possibile specificare qualsiasi valore stringa che distingue lo schema.</span><span class="sxs-lookup"><span data-stu-id="708f0-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="708f0-120">Schema di autenticazione dell'app è diverso dallo schema di autenticazione cookie dell'app.</span><span class="sxs-lookup"><span data-stu-id="708f0-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="708f0-121">Quando non viene fornito uno schema di autenticazione del cookie a <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, Usa `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").</span><span class="sxs-lookup"><span data-stu-id="708f0-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="708f0-122">Il cookie di autenticazione <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> è impostata su `true` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="708f0-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="708f0-123">I cookie di autenticazione sono consentiti quando un visitatore non ha fornito il consenso alla raccolta di dati.</span><span class="sxs-lookup"><span data-stu-id="708f0-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="708f0-124">Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="708f0-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="708f0-125">Nel `Startup.Configure` metodo, chiamare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che consente di impostare il `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="708f0-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="708f0-126">Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="708f0-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="708f0-127">Il <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="708f0-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="708f0-128">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="708f0-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="708f0-129">Middleware cookie criteri</span><span class="sxs-lookup"><span data-stu-id="708f0-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="708f0-130">[Middleware cookie criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) Abilita le funzionalità dei criteri di cookie.</span><span class="sxs-lookup"><span data-stu-id="708f0-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="708f0-131">Aggiungere il middleware alla pipeline di elaborazione delle app è ordine sensibili&mdash;riguarda solo i componenti a valle registrati nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="708f0-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="708f0-132">Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornito per il Middleware dei Cookie criteri per controllare caratteristiche globali del cookie elaborazione e di hook in elaborazione cookie gestori quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="708f0-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="708f0-133">Il valore predefinito <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> valore è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="708f0-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="708f0-134">Per applicare rigorosamente criteri nello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="708f0-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="708f0-135">Anche se questa impostazione interrompe OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di protezione dei cookie per altri tipi di App che non richiedono l'elaborazione della richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="708f0-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="708f0-136">L'impostazione di criteri Middleware dei Cookie per `MinimumSameSitePolicy` l'impostazione di `Cookie.SameSite` in `CookieAuthenticationOptions` impostazioni a seconda della matrice riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="708f0-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="708f0-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="708f0-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="708f0-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="708f0-138">Cookie.SameSite</span></span> | <span data-ttu-id="708f0-139">Impostazione Cookie.SameSite risultante</span><span class="sxs-lookup"><span data-stu-id="708f0-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="708f0-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="708f0-140">SameSiteMode.None</span></span>     | <span data-ttu-id="708f0-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="708f0-141">SameSiteMode.None</span></span><br><span data-ttu-id="708f0-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="708f0-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="708f0-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="708f0-144">SameSiteMode.None</span></span><br><span data-ttu-id="708f0-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="708f0-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="708f0-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="708f0-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="708f0-148">SameSiteMode.None</span></span><br><span data-ttu-id="708f0-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="708f0-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="708f0-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="708f0-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="708f0-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="708f0-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="708f0-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="708f0-155">SameSiteMode.None</span></span><br><span data-ttu-id="708f0-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="708f0-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="708f0-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="708f0-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="708f0-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="708f0-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="708f0-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="708f0-161">Creare un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="708f0-161">Create an authentication cookie</span></span>

<span data-ttu-id="708f0-162">Per creare un cookie che contiene informazioni sull'utente, costruire un <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="708f0-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="708f0-163">Le informazioni dell'utente vengano serializzate e archiviate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="708f0-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="708f0-164">Creare un <xref:System.Security.Claims.ClaimsIdentity> con le operazioni <xref:System.Security.Claims.Claim>s e chiamata <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per l'accesso dell'utente:</span><span class="sxs-lookup"><span data-stu-id="708f0-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="708f0-165">`SignInAsync` Crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="708f0-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="708f0-166">Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="708f0-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="708f0-167">ASP.NET Core [protezione dati](xref:security/data-protection/using-data-protection) sistema viene utilizzato per la crittografia.</span><span class="sxs-lookup"><span data-stu-id="708f0-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="708f0-168">Per un'app ospitata in più computer, di caricamento tramite una web farm, o bilanciamento del carico tra le app [configurare la protezione dati](xref:security/data-protection/configuration/overview) usare il Keyring stesso e l'identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="708f0-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="708f0-169">Esci</span><span class="sxs-lookup"><span data-stu-id="708f0-169">Sign out</span></span>

<span data-ttu-id="708f0-170">Per disconnettere l'utente corrente ed eliminare i cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="708f0-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="708f0-171">Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookie") non viene usato come lo schema (ad esempio, "ContosoCookie"), specificare lo schema utilizzato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="708f0-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="708f0-172">In caso contrario, viene usato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="708f0-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="708f0-173">Reagire alle modifiche di back-end</span><span class="sxs-lookup"><span data-stu-id="708f0-173">React to back-end changes</span></span>

<span data-ttu-id="708f0-174">Dopo aver creato un cookie, il cookie è l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="708f0-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="708f0-175">Se un account utente è disabilitato nei sistemi back-end:</span><span class="sxs-lookup"><span data-stu-id="708f0-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="708f0-176">Sistema di autenticazione cookie dell'app continua a elaborare le richieste basate su cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="708f0-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="708f0-177">L'utente rimane connesso all'App, purché il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="708f0-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="708f0-178">Il <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento può essere utilizzato per intercettare ed eseguire l'override di convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="708f0-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="708f0-179">Convalida il cookie a ogni richiesta riduce il rischio di revocato agli utenti l'accesso all'app.</span><span class="sxs-lookup"><span data-stu-id="708f0-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="708f0-180">Un approccio alla convalida dei cookie si basa su tenere traccia di quando il database utente viene modificato.</span><span class="sxs-lookup"><span data-stu-id="708f0-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="708f0-181">Se il database non è stato modificato perché è stato emesso il cookie dell'utente, non è necessario autenticare nuovamente l'utente se il cookie sia ancora valido.</span><span class="sxs-lookup"><span data-stu-id="708f0-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="708f0-182">Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un `LastChanged` valore.</span><span class="sxs-lookup"><span data-stu-id="708f0-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="708f0-183">Quando un utente viene aggiornato nel database, il `LastChanged` valore è impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="708f0-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="708f0-184">Per poter invalidare un cookie durante le modifiche del database in base il `LastChanged` valore, creare il cookie con un `LastChanged` attestazione contenente l'oggetto corrente `LastChanged` valore dal database:</span><span class="sxs-lookup"><span data-stu-id="708f0-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

<span data-ttu-id="708f0-185">Per implementare una sostituzione per il `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe che deriva da <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="708f0-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="708f0-186">Di seguito è riportato un esempio di implementazione di `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="708f0-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="708f0-187">Registrare l'istanza di eventi durante la registrazione del servizio nel cookie di `Startup.ConfigureServices` (metodo).</span><span class="sxs-lookup"><span data-stu-id="708f0-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="708f0-188">Fornire una [scoped la registrazione del servizio](xref:fundamentals/dependency-injection#service-lifetimes) per il `CustomCookieAuthenticationEvents` classe:</span><span class="sxs-lookup"><span data-stu-id="708f0-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="708f0-189">Si consideri una situazione in cui viene aggiornato il nome dell'utente&mdash;una decisione che non influiscono sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="708f0-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="708f0-190">Se si desidera aggiornare in modo non distruttivo l'entità utente, chiamare `context.ReplacePrincipal` e impostare il `context.ShouldRenew` proprietà `true`.</span><span class="sxs-lookup"><span data-stu-id="708f0-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="708f0-191">L'approccio descritto di seguito viene attivato a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="708f0-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="708f0-192">Convalida i cookie di autenticazione per tutti gli utenti a ogni richiesta può comportare una riduzione delle prestazioni di grandi dimensioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="708f0-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="708f0-193">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="708f0-193">Persistent cookies</span></span>

<span data-ttu-id="708f0-194">È possibile il cookie per rendere persistenti le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="708f0-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="708f0-195">La persistenza deve essere abilitata solo con il consenso dell'utente esplicita con una "Memorizza password" casella di controllo di accesso in o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="708f0-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="708f0-196">Il frammento di codice seguente crea un'identità e un corrispondente cookie in cui viene conservata tramite le chiusure del browser.</span><span class="sxs-lookup"><span data-stu-id="708f0-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="708f0-197">Vengono rispettate le impostazioni di scadenza scorrevole configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="708f0-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="708f0-198">Se il cookie scade mentre il browser viene chiuso, il browser Cancella il cookie dopo il riavvio.</span><span class="sxs-lookup"><span data-stu-id="708f0-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="708f0-199">Impostare <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> al `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="708f0-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="708f0-200">Scadenza del cookie assoluto</span><span class="sxs-lookup"><span data-stu-id="708f0-200">Absolute cookie expiration</span></span>

<span data-ttu-id="708f0-201">Un'ora di scadenza assoluto può essere impostata con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="708f0-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="708f0-202">Per creare un cookie permanente, `IsPersistent` deve essere impostata.</span><span class="sxs-lookup"><span data-stu-id="708f0-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="708f0-203">In caso contrario, il cookie viene creato con un ciclo di vita basati su sessione e far scadere prima o dopo il ticket di autenticazione che contiene.</span><span class="sxs-lookup"><span data-stu-id="708f0-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="708f0-204">Quando `ExpiresUtc` è impostato, sostituisce il valore della <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opzione di <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, se impostata.</span><span class="sxs-lookup"><span data-stu-id="708f0-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="708f0-205">Il frammento di codice seguente crea un'identità e un corrispondente cookie che dura per 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="708f0-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="708f0-206">Ignora eventuali impostazioni di scadenza scorrevole configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="708f0-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a><span data-ttu-id="708f0-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="708f0-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="708f0-208">Controlli di ruolo basata su criteri</span><span class="sxs-lookup"><span data-stu-id="708f0-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
