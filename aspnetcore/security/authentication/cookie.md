---
title: Usa autenticazione cookie senza ASP.NET Core identità
author: rick-anderson
description: Informazioni su come usare l'autenticazione tramite cookie senza ASP.NET Core identità.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
uid: security/authentication/cookie
ms.openlocfilehash: 76c7fc20c8870668ca7c65d975e2ed59f40f7dc8
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384825"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="3bca2-103">Usa autenticazione cookie senza ASP.NET Core identità</span><span class="sxs-lookup"><span data-stu-id="3bca2-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="3bca2-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3bca2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3bca2-105">ASP.NET Core identità è un provider di autenticazione completo e completo per la creazione e la gestione degli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="3bca2-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="3bca2-106">Tuttavia, è possibile usare un provider di autenticazione basata su cookie senza ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="3bca2-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="3bca2-107">Per altre informazioni, vedere <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="3bca2-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="3bca2-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3bca2-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3bca2-109">A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetico, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="3bca2-110">Usare l'indirizzo `maria.rodriguez@contoso.com` di **posta elettronica** e qualsiasi password per accedere all'utente.</span><span class="sxs-lookup"><span data-stu-id="3bca2-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="3bca2-111">L'utente viene autenticato nel `AuthenticateUser` metodo nel file *pages/account/login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="3bca2-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="3bca2-112">In un esempio reale, l'utente viene autenticato a fronte di un database.</span><span class="sxs-lookup"><span data-stu-id="3bca2-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="3bca2-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="3bca2-113">Configuration</span></span>

<span data-ttu-id="3bca2-114">Se l'app non usa il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il pacchetto [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="3bca2-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="3bca2-115">Nel metodo creare i servizi del middleware di autenticazione con i <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> metodi <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>e: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="3bca2-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="3bca2-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="3bca2-117">`AuthenticationScheme`è utile quando sono presenti più istanze dell'autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="3bca2-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="3bca2-118">L' `AuthenticationScheme` impostazione di su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) fornisce il valore "cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="3bca2-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="3bca2-119">È possibile specificare qualsiasi valore stringa che distingua lo schema.</span><span class="sxs-lookup"><span data-stu-id="3bca2-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="3bca2-120">Lo schema di autenticazione dell'app è diverso dallo schema di autenticazione dei cookie dell'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="3bca2-121">Quando non viene fornito uno schema di autenticazione <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>dei cookie a `CookieAuthenticationDefaults.AuthenticationScheme` , viene usato ("cookie").</span><span class="sxs-lookup"><span data-stu-id="3bca2-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="3bca2-122">La <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> proprietà del cookie di autenticazione è impostata `true` su per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3bca2-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="3bca2-123">I cookie di autenticazione sono consentiti quando un visitatore del sito non ha acconsentito alla raccolta dei dati.</span><span class="sxs-lookup"><span data-stu-id="3bca2-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="3bca2-124">Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="3bca2-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="3bca2-125">In `Startup.Configure`, chiamare `UseAuthentication` e `UseAuthorization` per impostare la `HttpContext.User` proprietà ed eseguire il middleware di autorizzazione per le richieste.</span><span class="sxs-lookup"><span data-stu-id="3bca2-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="3bca2-126">Chiamare i `UseAuthentication` metodi `UseAuthorization` e prima di `UseEndpoints`chiamare:</span><span class="sxs-lookup"><span data-stu-id="3bca2-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="3bca2-127">La <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bca2-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="3bca2-128">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione `Startup.ConfigureServices` nel metodo:</span><span class="sxs-lookup"><span data-stu-id="3bca2-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="3bca2-129">Middleware dei criteri dei cookie</span><span class="sxs-lookup"><span data-stu-id="3bca2-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="3bca2-130">Il [middleware dei criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) dei cookie Abilita le funzionalità dei criteri dei cookie.</span><span class="sxs-lookup"><span data-stu-id="3bca2-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="3bca2-131">L'aggiunta del middleware alla pipeline di elaborazione delle app è&mdash;sensibile agli ordini, ma interessa solo i componenti downstream registrati nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="3bca2-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="3bca2-132">Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornito al middleware dei criteri dei cookie per controllare le caratteristiche globali dell'elaborazione dei cookie e l'hook nei gestori di elaborazione dei cookie quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="3bca2-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="3bca2-133">Il valore <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> predefinito è `SameSiteMode.Lax` consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="3bca2-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="3bca2-134">Per applicare rigorosamente un criterio dello stesso sito `SameSiteMode.Strict`di, impostare `MinimumSameSitePolicy`il.</span><span class="sxs-lookup"><span data-stu-id="3bca2-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="3bca2-135">Sebbene questa impostazione interrompa OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di app che non si basano sull'elaborazione di richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="3bca2-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="3bca2-136">L'impostazione del middleware per i `MinimumSameSitePolicy` criteri dei cookie per può `Cookie.SameSite` influire sull'impostazione di in impostazioni in `CookieAuthenticationOptions` base alla matrice riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="3bca2-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="3bca2-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="3bca2-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="3bca2-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="3bca2-138">Cookie.SameSite</span></span> | <span data-ttu-id="3bca2-139">Impostazione cookie. navigava sullostesso sito risultante</span><span class="sxs-lookup"><span data-stu-id="3bca2-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="3bca2-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-140">SameSiteMode.None</span></span>     | <span data-ttu-id="3bca2-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-141">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="3bca2-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-144">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="3bca2-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="3bca2-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-148">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="3bca2-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="3bca2-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="3bca2-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-155">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="3bca2-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="3bca2-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="3bca2-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="3bca2-161">Creare un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="3bca2-161">Create an authentication cookie</span></span>

<span data-ttu-id="3bca2-162">Per creare un cookie che contiene informazioni sull'utente, <xref:System.Security.Claims.ClaimsPrincipal>costruire un oggetto.</span><span class="sxs-lookup"><span data-stu-id="3bca2-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="3bca2-163">Le informazioni utente vengono serializzate e archiviate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="3bca2-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="3bca2-164">Creare una <xref:System.Security.Claims.ClaimsIdentity> con qualsiasi richiesta <xref:System.Security.Claims.Claim>e chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per accedere all'utente:</span><span class="sxs-lookup"><span data-stu-id="3bca2-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="3bca2-165">`SignInAsync`Crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="3bca2-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="3bca2-166">Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bca2-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="3bca2-167">Per la crittografia viene usato il sistema di [protezione dei dati](xref:security/data-protection/using-data-protection) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3bca2-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="3bca2-168">Per un'app ospitata in più computer, bilanciamento del carico tra app o uso di un Web farm, [configurare la protezione dei dati](xref:security/data-protection/configuration/overview) in modo da usare lo stesso anello chiave e l'identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="3bca2-169">Esci</span><span class="sxs-lookup"><span data-stu-id="3bca2-169">Sign out</span></span>

<span data-ttu-id="3bca2-170">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="3bca2-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="3bca2-171">Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookie") non viene usato come schema (ad esempio, "ContosoCookie"), fornire lo schema usato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bca2-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="3bca2-172">In caso contrario, verrà utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bca2-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="3bca2-173">Reagire alle modifiche del back-end</span><span class="sxs-lookup"><span data-stu-id="3bca2-173">React to back-end changes</span></span>

<span data-ttu-id="3bca2-174">Una volta creato un cookie, il cookie è l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="3bca2-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="3bca2-175">Se un account utente è disabilitato nei sistemi back-end:</span><span class="sxs-lookup"><span data-stu-id="3bca2-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="3bca2-176">Il sistema di autenticazione dei cookie dell'app continua a elaborare le richieste in base al cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bca2-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="3bca2-177">L'utente rimane connesso all'app fino a quando il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="3bca2-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="3bca2-178">L' <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento può essere usato per intercettare ed eseguire l'override della convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="3bca2-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="3bca2-179">La convalida del cookie a ogni richiesta attenua il rischio di revocare gli utenti che accedono all'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="3bca2-180">Un approccio alla convalida dei cookie si basa sul tenere traccia del momento in cui il database utente viene modificato.</span><span class="sxs-lookup"><span data-stu-id="3bca2-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="3bca2-181">Se il database non è stato modificato dopo l'emissione del cookie dell'utente, non è necessario autenticare di nuovo l'utente se il cookie è ancora valido.</span><span class="sxs-lookup"><span data-stu-id="3bca2-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="3bca2-182">Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un `LastChanged` valore.</span><span class="sxs-lookup"><span data-stu-id="3bca2-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="3bca2-183">Quando un utente viene aggiornato nel database, il `LastChanged` valore viene impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="3bca2-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="3bca2-184">Per invalidare un cookie quando il database viene modificato in base al `LastChanged` valore, creare il cookie con un' `LastChanged` attestazione contenente il valore `LastChanged` corrente dal database:</span><span class="sxs-lookup"><span data-stu-id="3bca2-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="3bca2-185">Per implementare una sostituzione per l' `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe che deriva da: <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents></span><span class="sxs-lookup"><span data-stu-id="3bca2-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="3bca2-186">Di seguito è riportato un esempio di `CookieAuthenticationEvents`implementazione di:</span><span class="sxs-lookup"><span data-stu-id="3bca2-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="3bca2-187">Registrare l'istanza degli eventi durante la `Startup.ConfigureServices` registrazione del servizio cookie nel metodo.</span><span class="sxs-lookup"><span data-stu-id="3bca2-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3bca2-188">Fornire una [registrazione del servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes) per `CustomCookieAuthenticationEvents` la classe:</span><span class="sxs-lookup"><span data-stu-id="3bca2-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="3bca2-189">Si consideri una situazione in cui il nome dell'&mdash;utente viene aggiornato una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="3bca2-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="3bca2-190">Se si desidera aggiornare l'entità utente in modo non distruttivo, chiamare `context.ReplacePrincipal` e impostare la `context.ShouldRenew` proprietà su `true`.</span><span class="sxs-lookup"><span data-stu-id="3bca2-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3bca2-191">L'approccio descritto qui viene attivato a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="3bca2-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="3bca2-192">La convalida dei cookie di autenticazione per tutti gli utenti in ogni richiesta può comportare un notevole calo delle prestazioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="3bca2-193">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="3bca2-193">Persistent cookies</span></span>

<span data-ttu-id="3bca2-194">È possibile che il cookie venga mantenuto tra le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="3bca2-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="3bca2-195">Questa persistenza deve essere abilitata solo con il consenso esplicito dell'utente con una casella di controllo "Remember me" all'accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="3bca2-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="3bca2-196">Il frammento di codice seguente crea un'identità e un cookie corrispondente che sopravvivono attraverso le chiusure del browser.</span><span class="sxs-lookup"><span data-stu-id="3bca2-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="3bca2-197">Vengono rispettate tutte le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3bca2-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="3bca2-198">Se il cookie scade quando il browser viene chiuso, il cookie viene cancellato dopo il riavvio.</span><span class="sxs-lookup"><span data-stu-id="3bca2-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="3bca2-199">Imposta <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> su `true` in :<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties></span><span class="sxs-lookup"><span data-stu-id="3bca2-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="3bca2-200">Scadenza assoluta del cookie</span><span class="sxs-lookup"><span data-stu-id="3bca2-200">Absolute cookie expiration</span></span>

<span data-ttu-id="3bca2-201">È possibile impostare un'ora di scadenza assoluta <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>con.</span><span class="sxs-lookup"><span data-stu-id="3bca2-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="3bca2-202">Per creare un cookie persistente `IsPersistent` , è necessario impostare anche.</span><span class="sxs-lookup"><span data-stu-id="3bca2-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="3bca2-203">In caso contrario, il cookie viene creato con una durata basata sulla sessione e potrebbe scadere prima o dopo il ticket di autenticazione che possiede.</span><span class="sxs-lookup"><span data-stu-id="3bca2-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="3bca2-204">Quando `ExpiresUtc` è impostato, esegue l'override del valore <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> dell'opzione di <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, se impostata.</span><span class="sxs-lookup"><span data-stu-id="3bca2-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="3bca2-205">Il frammento di codice seguente crea un'identità e un cookie corrispondente che dura 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="3bca2-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="3bca2-206">Questa operazione ignora le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3bca2-206">This ignores any sliding expiration settings previously configured.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3bca2-207">ASP.NET Core identità è un provider di autenticazione completo e completo per la creazione e la gestione degli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="3bca2-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="3bca2-208">Tuttavia, è possibile usare un provider di autenticazione basata su cookie senza ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="3bca2-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="3bca2-209">Per altre informazioni, vedere <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="3bca2-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="3bca2-210">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3bca2-210">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3bca2-211">A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetico, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="3bca2-212">Usare l'indirizzo `maria.rodriguez@contoso.com` di **posta elettronica** e qualsiasi password per accedere all'utente.</span><span class="sxs-lookup"><span data-stu-id="3bca2-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="3bca2-213">L'utente viene autenticato nel `AuthenticateUser` metodo nel file *pages/account/login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="3bca2-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="3bca2-214">In un esempio reale, l'utente viene autenticato a fronte di un database.</span><span class="sxs-lookup"><span data-stu-id="3bca2-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="3bca2-215">Configurazione</span><span class="sxs-lookup"><span data-stu-id="3bca2-215">Configuration</span></span>

<span data-ttu-id="3bca2-216">Se l'app non usa il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il pacchetto [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="3bca2-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="3bca2-217">Nel metodo creare il servizio middleware di autenticazione con i <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> metodi e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="3bca2-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="3bca2-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="3bca2-219">`AuthenticationScheme`è utile quando sono presenti più istanze dell'autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="3bca2-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="3bca2-220">L' `AuthenticationScheme` impostazione di su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) fornisce il valore "cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="3bca2-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="3bca2-221">È possibile specificare qualsiasi valore stringa che distingua lo schema.</span><span class="sxs-lookup"><span data-stu-id="3bca2-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="3bca2-222">Lo schema di autenticazione dell'app è diverso dallo schema di autenticazione dei cookie dell'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="3bca2-223">Quando non viene fornito uno schema di autenticazione <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>dei cookie a `CookieAuthenticationDefaults.AuthenticationScheme` , viene usato ("cookie").</span><span class="sxs-lookup"><span data-stu-id="3bca2-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="3bca2-224">La <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> proprietà del cookie di autenticazione è impostata `true` su per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3bca2-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="3bca2-225">I cookie di autenticazione sono consentiti quando un visitatore del sito non ha acconsentito alla raccolta dei dati.</span><span class="sxs-lookup"><span data-stu-id="3bca2-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="3bca2-226">Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="3bca2-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="3bca2-227">Nel metodo chiamare il `UseAuthentication` metodo per richiamare il middleware di autenticazione che imposta la `HttpContext.User` proprietà. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="3bca2-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="3bca2-228">Chiamare il `UseAuthentication` metodo prima di `UseMvcWithDefaultRoute` chiamare `UseMvc`o:</span><span class="sxs-lookup"><span data-stu-id="3bca2-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="3bca2-229">La <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bca2-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="3bca2-230">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione `Startup.ConfigureServices` nel metodo:</span><span class="sxs-lookup"><span data-stu-id="3bca2-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="3bca2-231">Middleware dei criteri dei cookie</span><span class="sxs-lookup"><span data-stu-id="3bca2-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="3bca2-232">Il [middleware dei criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) dei cookie Abilita le funzionalità dei criteri dei cookie.</span><span class="sxs-lookup"><span data-stu-id="3bca2-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="3bca2-233">L'aggiunta del middleware alla pipeline di elaborazione delle app è&mdash;sensibile agli ordini, ma interessa solo i componenti downstream registrati nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="3bca2-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="3bca2-234">Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornito al middleware dei criteri dei cookie per controllare le caratteristiche globali dell'elaborazione dei cookie e l'hook nei gestori di elaborazione dei cookie quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="3bca2-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="3bca2-235">Il valore <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> predefinito è `SameSiteMode.Lax` consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="3bca2-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="3bca2-236">Per applicare rigorosamente un criterio dello stesso sito `SameSiteMode.Strict`di, impostare `MinimumSameSitePolicy`il.</span><span class="sxs-lookup"><span data-stu-id="3bca2-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="3bca2-237">Sebbene questa impostazione interrompa OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di app che non si basano sull'elaborazione di richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="3bca2-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="3bca2-238">L'impostazione del middleware per i `MinimumSameSitePolicy` criteri dei cookie per può `Cookie.SameSite` influire sull'impostazione di in impostazioni in `CookieAuthenticationOptions` base alla matrice riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="3bca2-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="3bca2-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="3bca2-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="3bca2-240">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="3bca2-240">Cookie.SameSite</span></span> | <span data-ttu-id="3bca2-241">Impostazione cookie. navigava sullostesso sito risultante</span><span class="sxs-lookup"><span data-stu-id="3bca2-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="3bca2-242">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-242">SameSiteMode.None</span></span>     | <span data-ttu-id="3bca2-243">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-243">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-244">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-245">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="3bca2-246">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-246">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-247">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-248">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="3bca2-249">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="3bca2-250">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-250">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-251">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-252">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="3bca2-253">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-254">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-255">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="3bca2-256">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="3bca2-257">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3bca2-257">SameSiteMode.None</span></span><br><span data-ttu-id="3bca2-258">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3bca2-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="3bca2-259">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="3bca2-260">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="3bca2-261">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="3bca2-262">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3bca2-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="3bca2-263">Creare un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="3bca2-263">Create an authentication cookie</span></span>

<span data-ttu-id="3bca2-264">Per creare un cookie che contiene informazioni sull'utente, <xref:System.Security.Claims.ClaimsPrincipal>costruire un oggetto.</span><span class="sxs-lookup"><span data-stu-id="3bca2-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="3bca2-265">Le informazioni utente vengono serializzate e archiviate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="3bca2-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="3bca2-266">Creare una <xref:System.Security.Claims.ClaimsIdentity> con qualsiasi richiesta <xref:System.Security.Claims.Claim>e chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per accedere all'utente:</span><span class="sxs-lookup"><span data-stu-id="3bca2-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="3bca2-267">`SignInAsync`Crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="3bca2-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="3bca2-268">Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bca2-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="3bca2-269">Per la crittografia viene usato il sistema di [protezione dei dati](xref:security/data-protection/using-data-protection) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3bca2-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="3bca2-270">Per un'app ospitata in più computer, bilanciamento del carico tra app o uso di un Web farm, [configurare la protezione dei dati](xref:security/data-protection/configuration/overview) in modo da usare lo stesso anello chiave e l'identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="3bca2-271">Esci</span><span class="sxs-lookup"><span data-stu-id="3bca2-271">Sign out</span></span>

<span data-ttu-id="3bca2-272">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="3bca2-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="3bca2-273">Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookie") non viene usato come schema (ad esempio, "ContosoCookie"), fornire lo schema usato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bca2-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="3bca2-274">In caso contrario, verrà utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bca2-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="3bca2-275">Reagire alle modifiche del back-end</span><span class="sxs-lookup"><span data-stu-id="3bca2-275">React to back-end changes</span></span>

<span data-ttu-id="3bca2-276">Una volta creato un cookie, il cookie è l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="3bca2-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="3bca2-277">Se un account utente è disabilitato nei sistemi back-end:</span><span class="sxs-lookup"><span data-stu-id="3bca2-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="3bca2-278">Il sistema di autenticazione dei cookie dell'app continua a elaborare le richieste in base al cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bca2-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="3bca2-279">L'utente rimane connesso all'app fino a quando il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="3bca2-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="3bca2-280">L' <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento può essere usato per intercettare ed eseguire l'override della convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="3bca2-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="3bca2-281">La convalida del cookie a ogni richiesta attenua il rischio di revocare gli utenti che accedono all'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="3bca2-282">Un approccio alla convalida dei cookie si basa sul tenere traccia del momento in cui il database utente viene modificato.</span><span class="sxs-lookup"><span data-stu-id="3bca2-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="3bca2-283">Se il database non è stato modificato dopo l'emissione del cookie dell'utente, non è necessario autenticare di nuovo l'utente se il cookie è ancora valido.</span><span class="sxs-lookup"><span data-stu-id="3bca2-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="3bca2-284">Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un `LastChanged` valore.</span><span class="sxs-lookup"><span data-stu-id="3bca2-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="3bca2-285">Quando un utente viene aggiornato nel database, il `LastChanged` valore viene impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="3bca2-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="3bca2-286">Per invalidare un cookie quando il database viene modificato in base al `LastChanged` valore, creare il cookie con un' `LastChanged` attestazione contenente il valore `LastChanged` corrente dal database:</span><span class="sxs-lookup"><span data-stu-id="3bca2-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="3bca2-287">Per implementare una sostituzione per l' `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe che deriva da: <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents></span><span class="sxs-lookup"><span data-stu-id="3bca2-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="3bca2-288">Di seguito è riportato un esempio di `CookieAuthenticationEvents`implementazione di:</span><span class="sxs-lookup"><span data-stu-id="3bca2-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="3bca2-289">Registrare l'istanza degli eventi durante la `Startup.ConfigureServices` registrazione del servizio cookie nel metodo.</span><span class="sxs-lookup"><span data-stu-id="3bca2-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3bca2-290">Fornire una [registrazione del servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes) per `CustomCookieAuthenticationEvents` la classe:</span><span class="sxs-lookup"><span data-stu-id="3bca2-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="3bca2-291">Si consideri una situazione in cui il nome dell'&mdash;utente viene aggiornato una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="3bca2-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="3bca2-292">Se si desidera aggiornare l'entità utente in modo non distruttivo, chiamare `context.ReplacePrincipal` e impostare la `context.ShouldRenew` proprietà su `true`.</span><span class="sxs-lookup"><span data-stu-id="3bca2-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3bca2-293">L'approccio descritto qui viene attivato a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="3bca2-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="3bca2-294">La convalida dei cookie di autenticazione per tutti gli utenti in ogni richiesta può comportare un notevole calo delle prestazioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="3bca2-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="3bca2-295">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="3bca2-295">Persistent cookies</span></span>

<span data-ttu-id="3bca2-296">È possibile che il cookie venga mantenuto tra le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="3bca2-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="3bca2-297">Questa persistenza deve essere abilitata solo con il consenso esplicito dell'utente con una casella di controllo "Remember me" all'accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="3bca2-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="3bca2-298">Il frammento di codice seguente crea un'identità e un cookie corrispondente che sopravvivono attraverso le chiusure del browser.</span><span class="sxs-lookup"><span data-stu-id="3bca2-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="3bca2-299">Vengono rispettate tutte le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3bca2-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="3bca2-300">Se il cookie scade quando il browser viene chiuso, il cookie viene cancellato dopo il riavvio.</span><span class="sxs-lookup"><span data-stu-id="3bca2-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="3bca2-301">Imposta <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> su `true` in :<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties></span><span class="sxs-lookup"><span data-stu-id="3bca2-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="3bca2-302">Scadenza assoluta del cookie</span><span class="sxs-lookup"><span data-stu-id="3bca2-302">Absolute cookie expiration</span></span>

<span data-ttu-id="3bca2-303">È possibile impostare un'ora di scadenza assoluta <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>con.</span><span class="sxs-lookup"><span data-stu-id="3bca2-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="3bca2-304">Per creare un cookie persistente `IsPersistent` , è necessario impostare anche.</span><span class="sxs-lookup"><span data-stu-id="3bca2-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="3bca2-305">In caso contrario, il cookie viene creato con una durata basata sulla sessione e potrebbe scadere prima o dopo il ticket di autenticazione che possiede.</span><span class="sxs-lookup"><span data-stu-id="3bca2-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="3bca2-306">Quando `ExpiresUtc` è impostato, esegue l'override del valore <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> dell'opzione di <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, se impostata.</span><span class="sxs-lookup"><span data-stu-id="3bca2-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="3bca2-307">Il frammento di codice seguente crea un'identità e un cookie corrispondente che dura 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="3bca2-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="3bca2-308">Questa operazione ignora le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3bca2-308">This ignores any sliding expiration settings previously configured.</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3bca2-309">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3bca2-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="3bca2-310">Controlli dei ruoli basati su criteri</span><span class="sxs-lookup"><span data-stu-id="3bca2-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
