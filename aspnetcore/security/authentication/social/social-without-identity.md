---
title: Facebook, Google e l'autenticazione tramite provider esterni senza ASP.NET Core Identity
author: rick-anderson
description: Spiegazione dell'uso di Facebook, Google, Twitter, autenticazione utente dell'account e così via senza ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561574"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Usare l'autenticazione tramite social provider Accedi senza ASP.NET Core Identity

<xref:security/authentication/social/index> viene descritto come consentire agli utenti di accedere usando OAuth 2.0 con le credenziali dai provider di autenticazione esterni. L'approccio descritto in questo argomento include ASP.NET Core Identity come provider di autenticazione.

In questo esempio illustra come usare un provider di autenticazione esterni **senza** ASP.NET Core Identity. Ciò è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core Identity, ma che richiedono l'integrazione con un provider di autenticazione esterno trusted.

Questo esempio viene utilizzata [l'autenticazione di Google](xref:security/authentication/google-logins) per l'autenticazione degli utenti. Usa Google authentication Sposta molte delle complessità di gestione del processo di accesso a Google. Per integrare con un provider di autenticazione esterni diversi, vedere gli argomenti seguenti:

* [Autenticazione Facebook](xref:security/authentication/facebook-logins)
* [Autenticazione Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticazione Twitter](xref:security/authentication/twitter-logins)
* [Altri provider](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configurazione

Nel `ConfigureServices` metodo, configurare gli schemi di autenticazione dell'app con il `AddAuthentication`, `AddCookie` e `AddGoogle` metodi:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

La chiamata a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) imposta dell'app [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme). Il `DefaultScheme` è lo schema predefinito utilizzato dalle seguenti `HttpContext` i metodi di estensione di autenticazione:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Impostazione dell'app `DefaultScheme` al [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookie") consente di configurare l'app per usare i cookie come lo schema predefinito per questi metodi di estensione. Impostazione dell'app <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> al [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") consente di configurare l'app per usare Google come lo schema predefinito per le chiamate a `ChallengeAsync`. `DefaultChallengeScheme` esegue l'override `DefaultScheme`. Visualizzare <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> per le proprietà aggiuntive che eseguono l'override `DefaultScheme` quando impostato.

Nel `Configure` metodo, chiamare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che consente di impostare il `HttpContext.User` proprietà. Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Per altre informazioni su schemi di autenticazione e l'autenticazione tramite cookie, vedere <xref:security/authentication/cookie>.

## <a name="applying-authorization"></a>Autorizzazione applicazione

Testare la configurazione di autenticazione dell'app applicando i `AuthorizeAttribute` attributo un controller, azioni o pagina. Il codice riportato di seguito limitano l'accesso per il *Privacy* pagina per gli utenti che sono stati autenticati:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Esci

Per disconnettere l'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0). Il codice seguente aggiunge un `Logout` gestore della pagina per il *indice* pagina:

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione. L'app `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` viene utilizzato come un fallback.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
