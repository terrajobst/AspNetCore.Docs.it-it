---
title: Usa autenticazione cookie senza ASP.NET Core identità
author: rick-anderson
description: Informazioni su come usare l'autenticazione tramite cookie senza ASP.NET Core identità.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 62a3d247dade6c83156a8378407d5e3891713fd1
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172113"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="4946a-103">Usa autenticazione cookie senza ASP.NET Core identità</span><span class="sxs-lookup"><span data-stu-id="4946a-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="4946a-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4946a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4946a-105">ASP.NET Core identità è un provider di autenticazione completo e completo per la creazione e la gestione degli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="4946a-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="4946a-106">Tuttavia, è possibile usare un provider di autenticazione basato su cookie senza ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="4946a-106">However, a cookie-based authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="4946a-107">Per altre informazioni, vedere <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="4946a-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="4946a-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4946a-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4946a-109">A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetico, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="4946a-110">Usare l'indirizzo di **posta elettronica** `maria.rodriguez@contoso.com` e qualsiasi password per accedere all'utente.</span><span class="sxs-lookup"><span data-stu-id="4946a-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="4946a-111">L'utente viene autenticato nel metodo `AuthenticateUser` nel file *pages/account/login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="4946a-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="4946a-112">In un esempio reale, l'utente viene autenticato a fronte di un database.</span><span class="sxs-lookup"><span data-stu-id="4946a-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="4946a-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="4946a-113">Configuration</span></span>

<span data-ttu-id="4946a-114">Se l'app non usa il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il pacchetto [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="4946a-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="4946a-115">Nel metodo `Startup.ConfigureServices` creare i servizi del middleware di autenticazione con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="4946a-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="4946a-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="4946a-117">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="4946a-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="4946a-118">Impostando il `AuthenticationScheme` su [CookieAuthenticationDefaults. AuthenticationScheme,](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) viene fornito il valore "cookies" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="4946a-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="4946a-119">È possibile specificare qualsiasi valore stringa che distingua lo schema.</span><span class="sxs-lookup"><span data-stu-id="4946a-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="4946a-120">Lo schema di autenticazione dell'app è diverso dallo schema di autenticazione dei cookie dell'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="4946a-121">Quando non viene fornito uno schema di autenticazione dei cookie per <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, viene usato `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").</span><span class="sxs-lookup"><span data-stu-id="4946a-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="4946a-122">Per impostazione predefinita, la proprietà <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> del cookie di autenticazione è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="4946a-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="4946a-123">I cookie di autenticazione sono consentiti quando un visitatore del sito non ha acconsentito alla raccolta dei dati.</span><span class="sxs-lookup"><span data-stu-id="4946a-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="4946a-124">Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="4946a-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="4946a-125">In `Startup.Configure`chiamare `UseAuthentication` e `UseAuthorization` per impostare la proprietà `HttpContext.User` ed eseguire il middleware di autorizzazione per le richieste.</span><span class="sxs-lookup"><span data-stu-id="4946a-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="4946a-126">Chiamare i metodi `UseAuthentication` e `UseAuthorization` prima di chiamare `UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="4946a-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="4946a-127">La classe <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4946a-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="4946a-128">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4946a-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="4946a-129">Middleware dei criteri dei cookie</span><span class="sxs-lookup"><span data-stu-id="4946a-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="4946a-130">Il [middleware dei criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) dei cookie Abilita le funzionalità dei criteri dei cookie.</span><span class="sxs-lookup"><span data-stu-id="4946a-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="4946a-131">L'aggiunta del middleware alla pipeline di elaborazione delle app è sensibile agli ordini&mdash;interessa solo i componenti downstream registrati nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="4946a-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="4946a-132">Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornite al middleware dei criteri dei cookie per controllare le caratteristiche globali dell'elaborazione dei cookie e l'hook nei gestori di elaborazione dei cookie quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="4946a-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="4946a-133">Il valore predefinito <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="4946a-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="4946a-134">Per applicare rigorosamente un criterio dello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="4946a-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="4946a-135">Sebbene questa impostazione interrompa OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di app che non si basano sull'elaborazione di richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="4946a-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="4946a-136">L'impostazione del middleware per i criteri dei cookie per `MinimumSameSitePolicy` può influire sull'impostazione di `Cookie.SameSite` nelle impostazioni `CookieAuthenticationOptions` in base alla matrice riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="4946a-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="4946a-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="4946a-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="4946a-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="4946a-138">Cookie.SameSite</span></span> | <span data-ttu-id="4946a-139">Impostazione cookie. navigava sullostesso sito risultante</span><span class="sxs-lookup"><span data-stu-id="4946a-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="4946a-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-140">SameSiteMode.None</span></span>     | <span data-ttu-id="4946a-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-141">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="4946a-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-144">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4946a-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="4946a-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-148">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="4946a-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4946a-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="4946a-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-155">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="4946a-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="4946a-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="4946a-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="4946a-161">Creare un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="4946a-161">Create an authentication cookie</span></span>

<span data-ttu-id="4946a-162">Per creare un cookie che contiene informazioni sull'utente, costruire un <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="4946a-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="4946a-163">Le informazioni utente vengono serializzate e archiviate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="4946a-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="4946a-164">Creare una <xref:System.Security.Claims.ClaimsIdentity> con qualsiasi <xref:System.Security.Claims.Claim>obbligatorio e chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per accedere all'utente:</span><span class="sxs-lookup"><span data-stu-id="4946a-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="4946a-165">`SignInAsync` crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="4946a-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="4946a-166">Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="4946a-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="4946a-167">Per la crittografia viene usato il sistema di [protezione dei dati](xref:security/data-protection/using-data-protection) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4946a-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="4946a-168">Per un'app ospitata in più computer, bilanciamento del carico tra app o uso di un Web farm, [configurare la protezione dei dati](xref:security/data-protection/configuration/overview) in modo da usare lo stesso anello chiave e l'identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="4946a-169">Disconnessione</span><span class="sxs-lookup"><span data-stu-id="4946a-169">Sign out</span></span>

<span data-ttu-id="4946a-170">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="4946a-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="4946a-171">Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookie") non viene utilizzato come schema (ad esempio, "ContosoCookie"), fornire lo schema utilizzato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4946a-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="4946a-172">In caso contrario, verrà utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="4946a-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="4946a-173">Reagire alle modifiche del back-end</span><span class="sxs-lookup"><span data-stu-id="4946a-173">React to back-end changes</span></span>

<span data-ttu-id="4946a-174">Una volta creato un cookie, il cookie è l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="4946a-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="4946a-175">Se un account utente è disabilitato nei sistemi back-end:</span><span class="sxs-lookup"><span data-stu-id="4946a-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="4946a-176">Il sistema di autenticazione dei cookie dell'app continua a elaborare le richieste in base al cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4946a-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="4946a-177">L'utente rimane connesso all'app fino a quando il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="4946a-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="4946a-178">È possibile utilizzare l'evento <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> per intercettare ed eseguire l'override della convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="4946a-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="4946a-179">La convalida del cookie a ogni richiesta attenua il rischio di revocare gli utenti che accedono all'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="4946a-180">Un approccio alla convalida dei cookie si basa sul tenere traccia del momento in cui il database utente viene modificato.</span><span class="sxs-lookup"><span data-stu-id="4946a-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="4946a-181">Se il database non è stato modificato dopo l'emissione del cookie dell'utente, non è necessario autenticare di nuovo l'utente se il cookie è ancora valido.</span><span class="sxs-lookup"><span data-stu-id="4946a-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="4946a-182">Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un valore `LastChanged`.</span><span class="sxs-lookup"><span data-stu-id="4946a-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="4946a-183">Quando un utente viene aggiornato nel database, il valore `LastChanged` viene impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="4946a-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="4946a-184">Per invalidare un cookie quando il database viene modificato in base al valore `LastChanged`, creare il cookie con un'attestazione `LastChanged` contenente il valore di `LastChanged` corrente dal database:</span><span class="sxs-lookup"><span data-stu-id="4946a-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="4946a-185">Per implementare una sostituzione per l'evento `ValidatePrincipal`, scrivere un metodo con la firma seguente in una classe che deriva da <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="4946a-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="4946a-186">Di seguito è riportato un esempio di implementazione di `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="4946a-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="4946a-187">Registrare l'istanza degli eventi durante la registrazione del servizio cookie nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4946a-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4946a-188">Fornire una [registrazione del servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes) per la classe `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="4946a-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="4946a-189">Si consideri una situazione in cui il nome dell'utente viene aggiornato&mdash;una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="4946a-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="4946a-190">Se si desidera aggiornare l'entità utente in modo non distruttivo, chiamare `context.ReplacePrincipal` e impostare la proprietà `context.ShouldRenew` su `true`.</span><span class="sxs-lookup"><span data-stu-id="4946a-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="4946a-191">L'approccio descritto qui viene attivato a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="4946a-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="4946a-192">La convalida dei cookie di autenticazione per tutti gli utenti in ogni richiesta può comportare un notevole calo delle prestazioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="4946a-193">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="4946a-193">Persistent cookies</span></span>

<span data-ttu-id="4946a-194">È possibile che il cookie venga mantenuto tra le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="4946a-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="4946a-195">Questa persistenza deve essere abilitata solo con il consenso esplicito dell'utente con una casella di controllo "Remember me" all'accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="4946a-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="4946a-196">Il frammento di codice seguente crea un'identità e un cookie corrispondente che sopravvivono attraverso le chiusure del browser.</span><span class="sxs-lookup"><span data-stu-id="4946a-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="4946a-197">Vengono rispettate tutte le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4946a-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="4946a-198">Se il cookie scade quando il browser viene chiuso, il cookie viene cancellato dopo il riavvio.</span><span class="sxs-lookup"><span data-stu-id="4946a-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="4946a-199">Impostare <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> su `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="4946a-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="4946a-200">Scadenza assoluta del cookie</span><span class="sxs-lookup"><span data-stu-id="4946a-200">Absolute cookie expiration</span></span>

<span data-ttu-id="4946a-201">È possibile impostare un'ora di scadenza assoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="4946a-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="4946a-202">Per creare un cookie persistente, è necessario impostare anche `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="4946a-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="4946a-203">In caso contrario, il cookie viene creato con una durata basata sulla sessione e potrebbe scadere prima o dopo il ticket di autenticazione che possiede.</span><span class="sxs-lookup"><span data-stu-id="4946a-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="4946a-204">Quando si imposta `ExpiresUtc`, viene eseguito l'override del valore dell'opzione <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> di <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, se impostata.</span><span class="sxs-lookup"><span data-stu-id="4946a-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="4946a-205">Il frammento di codice seguente crea un'identità e un cookie corrispondente che dura 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="4946a-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="4946a-206">Questa operazione ignora le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4946a-206">This ignores any sliding expiration settings previously configured.</span></span>

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

<span data-ttu-id="4946a-207">ASP.NET Core identità è un provider di autenticazione completo e completo per la creazione e la gestione degli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="4946a-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="4946a-208">Tuttavia, è possibile usare un provider di autenticazione basata su cookie senza ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="4946a-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="4946a-209">Per altre informazioni, vedere <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="4946a-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="4946a-210">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4946a-210">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4946a-211">A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetico, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="4946a-212">Usare l'indirizzo di **posta elettronica** `maria.rodriguez@contoso.com` e qualsiasi password per accedere all'utente.</span><span class="sxs-lookup"><span data-stu-id="4946a-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="4946a-213">L'utente viene autenticato nel metodo `AuthenticateUser` nel file *pages/account/login. cshtml. cs* .</span><span class="sxs-lookup"><span data-stu-id="4946a-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="4946a-214">In un esempio reale, l'utente viene autenticato a fronte di un database.</span><span class="sxs-lookup"><span data-stu-id="4946a-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="4946a-215">Configurazione</span><span class="sxs-lookup"><span data-stu-id="4946a-215">Configuration</span></span>

<span data-ttu-id="4946a-216">Se l'app non usa il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il pacchetto [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="4946a-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="4946a-217">Nel metodo `Startup.ConfigureServices` creare il servizio middleware di autenticazione con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="4946a-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="4946a-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="4946a-219">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="4946a-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="4946a-220">Impostando il `AuthenticationScheme` su [CookieAuthenticationDefaults. AuthenticationScheme,](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) viene fornito il valore "cookies" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="4946a-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="4946a-221">È possibile specificare qualsiasi valore stringa che distingua lo schema.</span><span class="sxs-lookup"><span data-stu-id="4946a-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="4946a-222">Lo schema di autenticazione dell'app è diverso dallo schema di autenticazione dei cookie dell'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="4946a-223">Quando non viene fornito uno schema di autenticazione dei cookie per <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, viene usato `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").</span><span class="sxs-lookup"><span data-stu-id="4946a-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="4946a-224">Per impostazione predefinita, la proprietà <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> del cookie di autenticazione è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="4946a-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="4946a-225">I cookie di autenticazione sono consentiti quando un visitatore del sito non ha acconsentito alla raccolta dei dati.</span><span class="sxs-lookup"><span data-stu-id="4946a-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="4946a-226">Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="4946a-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="4946a-227">Nel metodo `Startup.Configure` chiamare il metodo `UseAuthentication` per richiamare il middleware di autenticazione che imposta la proprietà `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="4946a-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="4946a-228">Chiamare il metodo `UseAuthentication` prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="4946a-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="4946a-229">La classe <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4946a-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="4946a-230">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4946a-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="4946a-231">Middleware dei criteri dei cookie</span><span class="sxs-lookup"><span data-stu-id="4946a-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="4946a-232">Il [middleware dei criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) dei cookie Abilita le funzionalità dei criteri dei cookie.</span><span class="sxs-lookup"><span data-stu-id="4946a-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="4946a-233">L'aggiunta del middleware alla pipeline di elaborazione delle app è sensibile agli ordini&mdash;interessa solo i componenti downstream registrati nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="4946a-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="4946a-234">Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornite al middleware dei criteri dei cookie per controllare le caratteristiche globali dell'elaborazione dei cookie e l'hook nei gestori di elaborazione dei cookie quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="4946a-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="4946a-235">Il valore predefinito <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="4946a-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="4946a-236">Per applicare rigorosamente un criterio dello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="4946a-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="4946a-237">Sebbene questa impostazione interrompa OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di app che non si basano sull'elaborazione di richieste tra origini.</span><span class="sxs-lookup"><span data-stu-id="4946a-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="4946a-238">L'impostazione del middleware per i criteri dei cookie per `MinimumSameSitePolicy` può influire sull'impostazione di `Cookie.SameSite` nelle impostazioni `CookieAuthenticationOptions` in base alla matrice riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="4946a-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="4946a-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="4946a-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="4946a-240">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="4946a-240">Cookie.SameSite</span></span> | <span data-ttu-id="4946a-241">Impostazione cookie. navigava sullostesso sito risultante</span><span class="sxs-lookup"><span data-stu-id="4946a-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="4946a-242">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-242">SameSiteMode.None</span></span>     | <span data-ttu-id="4946a-243">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-243">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-244">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-245">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="4946a-246">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-246">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-247">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-248">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4946a-249">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="4946a-250">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-250">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-251">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-252">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="4946a-253">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-254">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-255">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4946a-256">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="4946a-257">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4946a-257">SameSiteMode.None</span></span><br><span data-ttu-id="4946a-258">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4946a-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="4946a-259">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="4946a-260">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="4946a-261">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="4946a-262">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4946a-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="4946a-263">Creare un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="4946a-263">Create an authentication cookie</span></span>

<span data-ttu-id="4946a-264">Per creare un cookie che contiene informazioni sull'utente, costruire un <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="4946a-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="4946a-265">Le informazioni utente vengono serializzate e archiviate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="4946a-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="4946a-266">Creare una <xref:System.Security.Claims.ClaimsIdentity> con qualsiasi <xref:System.Security.Claims.Claim>obbligatorio e chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per accedere all'utente:</span><span class="sxs-lookup"><span data-stu-id="4946a-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="4946a-267">`SignInAsync` crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="4946a-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="4946a-268">Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="4946a-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="4946a-269">Per la crittografia viene usato il sistema di [protezione dei dati](xref:security/data-protection/using-data-protection) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4946a-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="4946a-270">Per un'app ospitata in più computer, bilanciamento del carico tra app o uso di un Web farm, [configurare la protezione dei dati](xref:security/data-protection/configuration/overview) in modo da usare lo stesso anello chiave e l'identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="4946a-271">Disconnessione</span><span class="sxs-lookup"><span data-stu-id="4946a-271">Sign out</span></span>

<span data-ttu-id="4946a-272">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="4946a-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="4946a-273">Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookie") non viene utilizzato come schema (ad esempio, "ContosoCookie"), fornire lo schema utilizzato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4946a-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="4946a-274">In caso contrario, verrà utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="4946a-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="4946a-275">Reagire alle modifiche del back-end</span><span class="sxs-lookup"><span data-stu-id="4946a-275">React to back-end changes</span></span>

<span data-ttu-id="4946a-276">Una volta creato un cookie, il cookie è l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="4946a-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="4946a-277">Se un account utente è disabilitato nei sistemi back-end:</span><span class="sxs-lookup"><span data-stu-id="4946a-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="4946a-278">Il sistema di autenticazione dei cookie dell'app continua a elaborare le richieste in base al cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4946a-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="4946a-279">L'utente rimane connesso all'app fino a quando il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="4946a-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="4946a-280">È possibile utilizzare l'evento <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> per intercettare ed eseguire l'override della convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="4946a-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="4946a-281">La convalida del cookie a ogni richiesta attenua il rischio di revocare gli utenti che accedono all'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="4946a-282">Un approccio alla convalida dei cookie si basa sul tenere traccia del momento in cui il database utente viene modificato.</span><span class="sxs-lookup"><span data-stu-id="4946a-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="4946a-283">Se il database non è stato modificato dopo l'emissione del cookie dell'utente, non è necessario autenticare di nuovo l'utente se il cookie è ancora valido.</span><span class="sxs-lookup"><span data-stu-id="4946a-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="4946a-284">Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un valore `LastChanged`.</span><span class="sxs-lookup"><span data-stu-id="4946a-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="4946a-285">Quando un utente viene aggiornato nel database, il valore `LastChanged` viene impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="4946a-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="4946a-286">Per invalidare un cookie quando il database viene modificato in base al valore `LastChanged`, creare il cookie con un'attestazione `LastChanged` contenente il valore di `LastChanged` corrente dal database:</span><span class="sxs-lookup"><span data-stu-id="4946a-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="4946a-287">Per implementare una sostituzione per l'evento `ValidatePrincipal`, scrivere un metodo con la firma seguente in una classe che deriva da <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="4946a-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="4946a-288">Di seguito è riportato un esempio di implementazione di `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="4946a-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="4946a-289">Registrare l'istanza degli eventi durante la registrazione del servizio cookie nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4946a-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4946a-290">Fornire una [registrazione del servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes) per la classe `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="4946a-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="4946a-291">Si consideri una situazione in cui il nome dell'utente viene aggiornato&mdash;una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="4946a-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="4946a-292">Se si desidera aggiornare l'entità utente in modo non distruttivo, chiamare `context.ReplacePrincipal` e impostare la proprietà `context.ShouldRenew` su `true`.</span><span class="sxs-lookup"><span data-stu-id="4946a-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="4946a-293">L'approccio descritto qui viene attivato a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="4946a-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="4946a-294">La convalida dei cookie di autenticazione per tutti gli utenti in ogni richiesta può comportare un notevole calo delle prestazioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="4946a-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="4946a-295">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="4946a-295">Persistent cookies</span></span>

<span data-ttu-id="4946a-296">È possibile che il cookie venga mantenuto tra le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="4946a-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="4946a-297">Questa persistenza deve essere abilitata solo con il consenso esplicito dell'utente con una casella di controllo "Remember me" all'accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="4946a-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="4946a-298">Il frammento di codice seguente crea un'identità e un cookie corrispondente che sopravvivono attraverso le chiusure del browser.</span><span class="sxs-lookup"><span data-stu-id="4946a-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="4946a-299">Vengono rispettate tutte le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4946a-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="4946a-300">Se il cookie scade quando il browser viene chiuso, il cookie viene cancellato dopo il riavvio.</span><span class="sxs-lookup"><span data-stu-id="4946a-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="4946a-301">Impostare <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> su `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="4946a-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="4946a-302">Scadenza assoluta del cookie</span><span class="sxs-lookup"><span data-stu-id="4946a-302">Absolute cookie expiration</span></span>

<span data-ttu-id="4946a-303">È possibile impostare un'ora di scadenza assoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="4946a-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="4946a-304">Per creare un cookie persistente, è necessario impostare anche `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="4946a-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="4946a-305">In caso contrario, il cookie viene creato con una durata basata sulla sessione e potrebbe scadere prima o dopo il ticket di autenticazione che possiede.</span><span class="sxs-lookup"><span data-stu-id="4946a-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="4946a-306">Quando si imposta `ExpiresUtc`, viene eseguito l'override del valore dell'opzione <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> di <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, se impostata.</span><span class="sxs-lookup"><span data-stu-id="4946a-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="4946a-307">Il frammento di codice seguente crea un'identità e un cookie corrispondente che dura 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="4946a-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="4946a-308">Questa operazione ignora le impostazioni di scadenza scorrevoli configurate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4946a-308">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="4946a-309">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4946a-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="4946a-310">Controlli dei ruoli basati su criteri</span><span class="sxs-lookup"><span data-stu-id="4946a-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
