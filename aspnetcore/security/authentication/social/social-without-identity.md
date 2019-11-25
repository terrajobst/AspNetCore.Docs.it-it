---
title: Autenticazione di Facebook, Google e del provider esterno senza ASP.NET Core identità
author: rick-anderson
description: Spiegazione dell'uso di Facebook, Google, Twitter e così via. autenticazione utente dell'account senza ASP.NET Core identità.
ms.author: riande
ms.date: 11/19/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 680ea091dcc5ed7f94879b5d277e8be7e5abeb7b
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289109"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="d397a-103">Usare l'autenticazione del provider di accesso di social networking senza ASP.NET Core identità</span><span class="sxs-lookup"><span data-stu-id="d397a-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d397a-104"><xref:security/authentication/social/index> descrive come consentire agli utenti di accedere con OAuth 2,0 con le credenziali dei provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="d397a-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="d397a-105">L'approccio descritto in questo argomento include ASP.NET Core identità come provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d397a-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="d397a-106">Questo esempio illustra come usare un provider di autenticazione esterno **senza** ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="d397a-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="d397a-107">Questa operazione è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core identità, ma richiedono comunque l'integrazione con un provider di autenticazione esterno attendibile.</span><span class="sxs-lookup"><span data-stu-id="d397a-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="d397a-108">Questo esempio usa [l'autenticazione di Google per l'](xref:security/authentication/google-logins) autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="d397a-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="d397a-109">L'uso dell'autenticazione Google sposta molte delle complessità della gestione del processo di accesso a Google.</span><span class="sxs-lookup"><span data-stu-id="d397a-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="d397a-110">Per l'integrazione con un provider di autenticazione esterno diverso, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d397a-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="d397a-111">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="d397a-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="d397a-112">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="d397a-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="d397a-113">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="d397a-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="d397a-114">Altri provider</span><span class="sxs-lookup"><span data-stu-id="d397a-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="d397a-115">Configurazione</span><span class="sxs-lookup"><span data-stu-id="d397a-115">Configuration</span></span>

<span data-ttu-id="d397a-116">Nel metodo `ConfigureServices` configurare gli schemi di autenticazione dell'app con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>e <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:</span><span class="sxs-lookup"><span data-stu-id="d397a-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

<span data-ttu-id="d397a-117">La chiamata a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> imposta l'<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>dell'app.</span><span class="sxs-lookup"><span data-stu-id="d397a-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="d397a-118">Il `DefaultScheme` è lo schema predefinito utilizzato dai seguenti metodi di estensione di autenticazione `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="d397a-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="d397a-119">Impostando il `DefaultScheme` dell'app su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), l'app viene configurata in modo da usare i cookie come schema predefinito per questi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="d397a-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="d397a-120">Impostando il <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> dell'app su [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), l'app viene configurata in modo da usare Google come schema predefinito per le chiamate ai `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d397a-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="d397a-121">`DefaultChallengeScheme` esegue l'override di `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="d397a-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="d397a-122">Vedere <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> per proprietà aggiuntive che eseguono l'override di `DefaultScheme` quando impostate.</span><span class="sxs-lookup"><span data-stu-id="d397a-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="d397a-123">In `Startup.Configure`chiamare `UseAuthentication` e `UseAuthorization` tra la chiamata di `UseRouting` e di `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="d397a-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` between calling `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="d397a-124">Viene impostata la proprietà `HttpContext.User` ed eseguito il middleware di autorizzazione per le richieste:</span><span class="sxs-lookup"><span data-stu-id="d397a-124">This sets the `HttpContext.User` property and runs the Authorization Middleware for requests:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

<span data-ttu-id="d397a-125">Per ulteriori informazioni sugli schemi di autenticazione e sull'autenticazione dei cookie, vedere <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="d397a-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="d397a-126">Applica autorizzazione</span><span class="sxs-lookup"><span data-stu-id="d397a-126">Apply authorization</span></span>

<span data-ttu-id="d397a-127">Testare la configurazione di autenticazione dell'app applicando l'attributo `AuthorizeAttribute` a un controller, un'azione o una pagina.</span><span class="sxs-lookup"><span data-stu-id="d397a-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="d397a-128">Il codice seguente limita l'accesso alla pagina *privacy* agli utenti che sono stati autenticati:</span><span class="sxs-lookup"><span data-stu-id="d397a-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="d397a-129">Esci</span><span class="sxs-lookup"><span data-stu-id="d397a-129">Sign out</span></span>

<span data-ttu-id="d397a-130">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="d397a-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="d397a-131">Il codice seguente aggiunge un gestore di pagine `Logout` alla pagina di *Indice* :</span><span class="sxs-lookup"><span data-stu-id="d397a-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="d397a-132">Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d397a-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="d397a-133">Il `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` dell'app viene usato come fallback.</span><span class="sxs-lookup"><span data-stu-id="d397a-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d397a-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d397a-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d397a-135"><xref:security/authentication/social/index> descrive come consentire agli utenti di accedere con OAuth 2,0 con le credenziali dei provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="d397a-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="d397a-136">L'approccio descritto in questo argomento include ASP.NET Core identità come provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d397a-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="d397a-137">Questo esempio illustra come usare un provider di autenticazione esterno **senza** ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="d397a-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="d397a-138">Questa operazione è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core identità, ma richiedono comunque l'integrazione con un provider di autenticazione esterno attendibile.</span><span class="sxs-lookup"><span data-stu-id="d397a-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="d397a-139">Questo esempio usa [l'autenticazione di Google per l'](xref:security/authentication/google-logins) autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="d397a-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="d397a-140">L'uso dell'autenticazione Google sposta molte delle complessità della gestione del processo di accesso a Google.</span><span class="sxs-lookup"><span data-stu-id="d397a-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="d397a-141">Per l'integrazione con un provider di autenticazione esterno diverso, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d397a-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="d397a-142">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="d397a-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="d397a-143">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="d397a-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="d397a-144">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="d397a-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="d397a-145">Altri provider</span><span class="sxs-lookup"><span data-stu-id="d397a-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="d397a-146">Configurazione</span><span class="sxs-lookup"><span data-stu-id="d397a-146">Configuration</span></span>

<span data-ttu-id="d397a-147">Nel metodo `ConfigureServices` configurare gli schemi di autenticazione dell'app con i metodi `AddAuthentication`, `AddCookie`e `AddGoogle`:</span><span class="sxs-lookup"><span data-stu-id="d397a-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="d397a-148">La chiamata a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) imposta l' [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)dell'app.</span><span class="sxs-lookup"><span data-stu-id="d397a-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="d397a-149">Il `DefaultScheme` è lo schema predefinito utilizzato dai seguenti metodi di estensione di autenticazione `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="d397a-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="d397a-150">Impostando il `DefaultScheme` dell'app su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), l'app viene configurata in modo da usare i cookie come schema predefinito per questi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="d397a-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="d397a-151">Impostando il <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> dell'app su [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), l'app viene configurata in modo da usare Google come schema predefinito per le chiamate ai `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d397a-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="d397a-152">`DefaultChallengeScheme` esegue l'override di `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="d397a-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="d397a-153">Vedere <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> per proprietà aggiuntive che eseguono l'override di `DefaultScheme` quando impostate.</span><span class="sxs-lookup"><span data-stu-id="d397a-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="d397a-154">Nel metodo `Configure` chiamare il metodo `UseAuthentication` per richiamare il middleware di autenticazione che imposta la proprietà `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="d397a-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="d397a-155">Chiamare il metodo `UseAuthentication` prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="d397a-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

<span data-ttu-id="d397a-156">Per ulteriori informazioni sugli schemi di autenticazione e sull'autenticazione dei cookie, vedere <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="d397a-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="d397a-157">Applica autorizzazione</span><span class="sxs-lookup"><span data-stu-id="d397a-157">Apply authorization</span></span>

<span data-ttu-id="d397a-158">Testare la configurazione di autenticazione dell'app applicando l'attributo `AuthorizeAttribute` a un controller, un'azione o una pagina.</span><span class="sxs-lookup"><span data-stu-id="d397a-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="d397a-159">Il codice seguente limita l'accesso alla pagina *privacy* agli utenti che sono stati autenticati:</span><span class="sxs-lookup"><span data-stu-id="d397a-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="d397a-160">Esci</span><span class="sxs-lookup"><span data-stu-id="d397a-160">Sign out</span></span>

<span data-ttu-id="d397a-161">Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="d397a-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="d397a-162">Il codice seguente aggiunge un gestore di pagine `Logout` alla pagina di *Indice* :</span><span class="sxs-lookup"><span data-stu-id="d397a-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="d397a-163">Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d397a-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="d397a-164">Il `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` dell'app viene usato come fallback.</span><span class="sxs-lookup"><span data-stu-id="d397a-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d397a-165">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d397a-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
