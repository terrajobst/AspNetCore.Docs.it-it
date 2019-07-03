---
title: Facebook, Google e l'autenticazione tramite provider esterni senza ASP.NET Core Identity
author: rick-anderson
description: Spiegazione dell'uso di Facebook, Google, Twitter, autenticazione utente dell'account e così via senza ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: e67da513fef1ce453110c465b08e9c7965e71df5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557650"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="10c6d-103">Usare l'autenticazione tramite social provider Accedi senza ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="10c6d-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="10c6d-104"><xref:security/authentication/social/index> viene descritto come consentire agli utenti di accedere usando OAuth 2.0 con le credenziali dai provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="10c6d-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="10c6d-105">L'approccio descritto in questo argomento include ASP.NET Core Identity come provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="10c6d-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="10c6d-106">In questo esempio illustra come usare un provider di autenticazione esterni **senza** ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="10c6d-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="10c6d-107">Ciò è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core Identity, ma che richiedono l'integrazione con un provider di autenticazione esterno trusted.</span><span class="sxs-lookup"><span data-stu-id="10c6d-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="10c6d-108">Questo esempio viene utilizzata [l'autenticazione di Google](xref:security/authentication/google-logins) per l'autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="10c6d-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="10c6d-109">Usa Google authentication Sposta molte delle complessità di gestione del processo di accesso a Google.</span><span class="sxs-lookup"><span data-stu-id="10c6d-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="10c6d-110">Per integrare con un provider di autenticazione esterni diversi, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="10c6d-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="10c6d-111">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="10c6d-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="10c6d-112">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="10c6d-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="10c6d-113">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="10c6d-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="10c6d-114">Altri provider</span><span class="sxs-lookup"><span data-stu-id="10c6d-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="10c6d-115">Configurazione</span><span class="sxs-lookup"><span data-stu-id="10c6d-115">Configuration</span></span>

<span data-ttu-id="10c6d-116">Nel `ConfigureServices` metodo, configurare gli schemi di autenticazione dell'app con il `AddAuthentication`, `AddCookie` e `AddGoogle` metodi:</span><span class="sxs-lookup"><span data-stu-id="10c6d-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="10c6d-117">La chiamata a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) imposta dell'app [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span><span class="sxs-lookup"><span data-stu-id="10c6d-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="10c6d-118">Il `DefaultScheme` è lo schema predefinito utilizzato dalle seguenti `HttpContext` i metodi di estensione di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="10c6d-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="10c6d-119">Impostazione dell'app `DefaultScheme` al [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookie") consente di configurare l'app per usare i cookie come lo schema predefinito per questi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="10c6d-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="10c6d-120">Impostazione dell'app <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> al [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") consente di configurare l'app per usare Google come lo schema predefinito per le chiamate a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="10c6d-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="10c6d-121">`DefaultChallengeScheme` esegue l'override `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="10c6d-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="10c6d-122">Visualizzare <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> per le proprietà aggiuntive che eseguono l'override `DefaultScheme` quando impostato.</span><span class="sxs-lookup"><span data-stu-id="10c6d-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="10c6d-123">Nel `Configure` metodo, chiamare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che consente di impostare il `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="10c6d-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="10c6d-124">Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="10c6d-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="10c6d-125">Per altre informazioni su schemi di autenticazione e l'autenticazione tramite cookie, vedere <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="10c6d-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-basic-authorization"></a><span data-ttu-id="10c6d-126">Applicazione di autorizzazione di base</span><span class="sxs-lookup"><span data-stu-id="10c6d-126">Applying basic authorization</span></span>

<span data-ttu-id="10c6d-127">Testare la configurazione di autenticazione dell'app applicando i `AuthorizeAttribute` attributo un controller, azioni o pagina.</span><span class="sxs-lookup"><span data-stu-id="10c6d-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="10c6d-128">Il codice riportato di seguito limitano l'accesso per il *Privacy* pagina per gli utenti che sono stati autenticati:</span><span class="sxs-lookup"><span data-stu-id="10c6d-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="10c6d-129">Esci</span><span class="sxs-lookup"><span data-stu-id="10c6d-129">Sign out</span></span>

<span data-ttu-id="10c6d-130">Per disconnettere l'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="10c6d-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="10c6d-131">Il codice seguente aggiunge un `Logout` gestore della pagina per il *indice* pagina:</span><span class="sxs-lookup"><span data-stu-id="10c6d-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="10c6d-132">Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="10c6d-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="10c6d-133">L'app `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` viene utilizzato come un fallback.</span><span class="sxs-lookup"><span data-stu-id="10c6d-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10c6d-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="10c6d-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
