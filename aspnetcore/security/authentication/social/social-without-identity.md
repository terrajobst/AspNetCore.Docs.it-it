---
title: Autenticazione di Facebook, Google e del provider esterno senza ASP.NET Core identità
author: rick-anderson
description: Spiegazione dell'uso di Facebook, Google, Twitter e così via. autenticazione utente dell'account senza ASP.NET Core identità.
ms.author: riande
ms.date: 09/25/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 54dd93a13b2f7ed09c2c305f529d5f4610567184
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999890"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="e7990-103">Usare l'autenticazione del provider di accesso di social networking senza ASP.NET Core identità</span><span class="sxs-lookup"><span data-stu-id="e7990-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e7990-104"><xref:security/authentication/social/index> descrive come consentire agli utenti di accedere con OAuth 2,0 con le credenziali dei provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="e7990-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="e7990-105">L'approccio descritto in questo argomento include ASP.NET Core identità come provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e7990-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="e7990-106">Questo esempio illustra come usare un provider di autenticazione esterno **senza** ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="e7990-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="e7990-107">Questa operazione è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core identità, ma richiedono comunque l'integrazione con un provider di autenticazione esterno attendibile.</span><span class="sxs-lookup"><span data-stu-id="e7990-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="e7990-108">Questo esempio usa [l'autenticazione di Google per l'](xref:security/authentication/google-logins) autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="e7990-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="e7990-109">L'uso dell'autenticazione Google sposta molte delle complessità della gestione del processo di accesso a Google.</span><span class="sxs-lookup"><span data-stu-id="e7990-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="e7990-110">Per l'integrazione con un provider di autenticazione esterno diverso, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="e7990-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="e7990-111">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="e7990-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="e7990-112">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="e7990-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="e7990-113">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="e7990-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="e7990-114">Altri provider</span><span class="sxs-lookup"><span data-stu-id="e7990-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="e7990-115">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e7990-115">Configuration</span></span>

<span data-ttu-id="e7990-116">Nel metodo `ConfigureServices` configurare gli schemi di autenticazione dell'app con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> e <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:</span><span class="sxs-lookup"><span data-stu-id="e7990-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e7990-117">La chiamata a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> imposta <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme> dell'app.</span><span class="sxs-lookup"><span data-stu-id="e7990-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="e7990-118">Il `DefaultScheme` è lo schema predefinito utilizzato dai seguenti metodi di estensione per l'autenticazione `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="e7990-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="e7990-119">Impostando il `DefaultScheme` dell'app su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), l'app viene configurata in modo da usare i cookie come schema predefinito per questi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="e7990-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="e7990-120">Impostando il <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> dell'app su [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), l'app viene configurata in modo da usare Google come schema predefinito per le chiamate a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7990-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="e7990-121">`DefaultChallengeScheme` esegue l'override di `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="e7990-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="e7990-122">Per ulteriori proprietà che eseguono l'override di `DefaultScheme` se impostate, vedere <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="e7990-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="e7990-123">In `Startup.Configure` chiamare `UseAuthentication` e `UseAuthorization` per impostare la proprietà `HttpContext.User` ed eseguire il middleware di autorizzazione per le richieste.</span><span class="sxs-lookup"><span data-stu-id="e7990-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="e7990-124">Chiamare i metodi `UseAuthentication` e `UseAuthorization` prima di chiamare `UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="e7990-124">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

<span data-ttu-id="e7990-125">Per ulteriori informazioni sugli schemi di autenticazione e sull'autenticazione dei cookie, vedere <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="e7990-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="e7990-126">Applica autorizzazione</span><span class="sxs-lookup"><span data-stu-id="e7990-126">Apply authorization</span></span>

<span data-ttu-id="e7990-127">Testare la configurazione di autenticazione dell'app applicando l'attributo `AuthorizeAttribute` a un controller, un'azione o una pagina.</span><span class="sxs-lookup"><span data-stu-id="e7990-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="e7990-128">Il codice seguente limita l'accesso alla pagina *privacy* agli utenti che sono stati autenticati:</span><span class="sxs-lookup"><span data-stu-id="e7990-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="e7990-129">Esci</span><span class="sxs-lookup"><span data-stu-id="e7990-129">Sign out</span></span>

<span data-ttu-id="e7990-130">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="e7990-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="e7990-131">Il codice seguente aggiunge un gestore di pagine `Logout` alla pagina di *Indice* :</span><span class="sxs-lookup"><span data-stu-id="e7990-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

<span data-ttu-id="e7990-132">Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e7990-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="e7990-133">L'app `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` viene usata come fallback.</span><span class="sxs-lookup"><span data-stu-id="e7990-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7990-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e7990-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e7990-135"><xref:security/authentication/social/index> descrive come consentire agli utenti di accedere con OAuth 2,0 con le credenziali dei provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="e7990-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="e7990-136">L'approccio descritto in questo argomento include ASP.NET Core identità come provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e7990-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="e7990-137">Questo esempio illustra come usare un provider di autenticazione esterno **senza** ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="e7990-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="e7990-138">Questa operazione è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core identità, ma richiedono comunque l'integrazione con un provider di autenticazione esterno attendibile.</span><span class="sxs-lookup"><span data-stu-id="e7990-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="e7990-139">Questo esempio usa [l'autenticazione di Google per l'](xref:security/authentication/google-logins) autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="e7990-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="e7990-140">L'uso dell'autenticazione Google sposta molte delle complessità della gestione del processo di accesso a Google.</span><span class="sxs-lookup"><span data-stu-id="e7990-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="e7990-141">Per l'integrazione con un provider di autenticazione esterno diverso, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="e7990-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="e7990-142">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="e7990-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="e7990-143">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="e7990-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="e7990-144">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="e7990-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="e7990-145">Altri provider</span><span class="sxs-lookup"><span data-stu-id="e7990-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="e7990-146">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e7990-146">Configuration</span></span>

<span data-ttu-id="e7990-147">Nel metodo `ConfigureServices` configurare gli schemi di autenticazione dell'app con i metodi `AddAuthentication`, `AddCookie` e `AddGoogle`:</span><span class="sxs-lookup"><span data-stu-id="e7990-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e7990-148">La chiamata a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) imposta l' [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)dell'app.</span><span class="sxs-lookup"><span data-stu-id="e7990-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="e7990-149">Il `DefaultScheme` è lo schema predefinito utilizzato dai seguenti metodi di estensione per l'autenticazione `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="e7990-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="e7990-150">Impostando il `DefaultScheme` dell'app su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), l'app viene configurata in modo da usare i cookie come schema predefinito per questi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="e7990-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="e7990-151">Impostando il <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> dell'app su [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), l'app viene configurata in modo da usare Google come schema predefinito per le chiamate a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7990-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="e7990-152">`DefaultChallengeScheme` esegue l'override di `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="e7990-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="e7990-153">Per ulteriori proprietà che eseguono l'override di `DefaultScheme` se impostate, vedere <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="e7990-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="e7990-154">Nel metodo `Configure` chiamare il metodo `UseAuthentication` per richiamare il middleware di autenticazione che imposta la proprietà `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="e7990-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="e7990-155">Chiamare il metodo `UseAuthentication` prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="e7990-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="e7990-156">Per ulteriori informazioni sugli schemi di autenticazione e sull'autenticazione dei cookie, vedere <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="e7990-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="e7990-157">Applica autorizzazione</span><span class="sxs-lookup"><span data-stu-id="e7990-157">Apply authorization</span></span>

<span data-ttu-id="e7990-158">Testare la configurazione di autenticazione dell'app applicando l'attributo `AuthorizeAttribute` a un controller, un'azione o una pagina.</span><span class="sxs-lookup"><span data-stu-id="e7990-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="e7990-159">Il codice seguente limita l'accesso alla pagina *privacy* agli utenti che sono stati autenticati:</span><span class="sxs-lookup"><span data-stu-id="e7990-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="e7990-160">Esci</span><span class="sxs-lookup"><span data-stu-id="e7990-160">Sign out</span></span>

<span data-ttu-id="e7990-161">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="e7990-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="e7990-162">Il codice seguente aggiunge un gestore di pagine `Logout` alla pagina di *Indice* :</span><span class="sxs-lookup"><span data-stu-id="e7990-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="e7990-163">Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e7990-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="e7990-164">L'app `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` viene usata come fallback.</span><span class="sxs-lookup"><span data-stu-id="e7990-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7990-165">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e7990-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
