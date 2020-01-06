---
title: Autenticazione di Facebook, Google e del provider esterno senza ASP.NET Core identità
author: rick-anderson
description: Spiegazione dell'uso di Facebook, Google, Twitter e così via. autenticazione utente dell'account senza ASP.NET Core identità.
ms.author: riande
ms.date: 12/10/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 612964ec9ed4975cdc81780dda3bac6cce96037f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75359058"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Usare l'autenticazione del provider di accesso di social networking senza ASP.NET Core identità

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> descrive come consentire agli utenti di accedere con OAuth 2,0 con le credenziali dei provider di autenticazione esterni. L'approccio descritto in questo argomento include ASP.NET Core identità come provider di autenticazione.

Questo esempio illustra come usare un provider di autenticazione esterno **senza** ASP.NET Core identità. Questa operazione è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core identità, ma richiedono comunque l'integrazione con un provider di autenticazione esterno attendibile.

Questo esempio usa [l'autenticazione di Google per l'](xref:security/authentication/google-logins) autenticazione degli utenti. L'uso dell'autenticazione Google sposta molte delle complessità della gestione del processo di accesso a Google. Per l'integrazione con un provider di autenticazione esterno diverso, vedere gli argomenti seguenti:

* [Autenticazione Facebook](xref:security/authentication/facebook-logins)
* [Autenticazione Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticazione Twitter](xref:security/authentication/twitter-logins)
* [Altri provider](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configurazione di

Nel metodo `ConfigureServices` configurare gli schemi di autenticazione dell'app con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>e <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

La chiamata a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> imposta l'<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>dell'app. Il `DefaultScheme` è lo schema predefinito utilizzato dai seguenti metodi di estensione di autenticazione `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Impostando il `DefaultScheme` dell'app su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), l'app viene configurata in modo da usare i cookie come schema predefinito per questi metodi di estensione. Impostando il <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> dell'app su [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), l'app viene configurata in modo da usare Google come schema predefinito per le chiamate ai `ChallengeAsync`. `DefaultChallengeScheme` esegue l'override di `DefaultScheme`. Vedere <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> per proprietà aggiuntive che eseguono l'override di `DefaultScheme` quando impostate.

In `Startup.Configure`chiamare `UseAuthentication` e `UseAuthorization` tra la chiamata di `UseRouting` e di `UseEndpoints`. Viene impostata la proprietà `HttpContext.User` ed eseguito il middleware di autorizzazione per le richieste:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

Per altre informazioni sugli schemi di autenticazione, vedere [concetti relativi all'autenticazione](xref:security/authentication/index#authentication-concepts). Per ulteriori informazioni sull'autenticazione dei cookie, vedere <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Applica autorizzazione

Testare la configurazione di autenticazione dell'app applicando l'attributo `AuthorizeAttribute` a un controller, un'azione o una pagina. Il codice seguente limita l'accesso alla pagina *privacy* agli utenti che sono stati autenticati:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Esci

Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Il codice seguente aggiunge un gestore di pagine `Logout` alla pagina di *Indice* :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione. Il `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` dell'app viene usato come fallback.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> descrive come consentire agli utenti di accedere con OAuth 2,0 con le credenziali dei provider di autenticazione esterni. L'approccio descritto in questo argomento include ASP.NET Core identità come provider di autenticazione.

Questo esempio illustra come usare un provider di autenticazione esterno **senza** ASP.NET Core identità. Questa operazione è utile per le app che non richiedono tutte le funzionalità di ASP.NET Core identità, ma richiedono comunque l'integrazione con un provider di autenticazione esterno attendibile.

Questo esempio usa [l'autenticazione di Google per l'](xref:security/authentication/google-logins) autenticazione degli utenti. L'uso dell'autenticazione Google sposta molte delle complessità della gestione del processo di accesso a Google. Per l'integrazione con un provider di autenticazione esterno diverso, vedere gli argomenti seguenti:

* [Autenticazione Facebook](xref:security/authentication/facebook-logins)
* [Autenticazione Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticazione Twitter](xref:security/authentication/twitter-logins)
* [Altri provider](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configurazione di

Nel metodo `ConfigureServices` configurare gli schemi di autenticazione dell'app con i metodi `AddAuthentication`, `AddCookie`e `AddGoogle`:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

La chiamata a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) imposta l' [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)dell'app. Il `DefaultScheme` è lo schema predefinito utilizzato dai seguenti metodi di estensione di autenticazione `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Impostando il `DefaultScheme` dell'app su [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), l'app viene configurata in modo da usare i cookie come schema predefinito per questi metodi di estensione. Impostando il <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> dell'app su [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), l'app viene configurata in modo da usare Google come schema predefinito per le chiamate ai `ChallengeAsync`. `DefaultChallengeScheme` esegue l'override di `DefaultScheme`. Vedere <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> per proprietà aggiuntive che eseguono l'override di `DefaultScheme` quando impostate.

Nel metodo `Configure` chiamare il metodo `UseAuthentication` per richiamare il middleware di autenticazione che imposta la proprietà `HttpContext.User`. Chiamare il metodo `UseAuthentication` prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

Per altre informazioni sugli schemi di autenticazione, vedere [concetti relativi all'autenticazione](xref:security/authentication/index#authentication-concepts). Per ulteriori informazioni sull'autenticazione dei cookie, vedere <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Applica autorizzazione

Testare la configurazione di autenticazione dell'app applicando l'attributo `AuthorizeAttribute` a un controller, un'azione o una pagina. Il codice seguente limita l'accesso alla pagina *privacy* agli utenti che sono stati autenticati:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Esci

Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Il codice seguente aggiunge un gestore di pagine `Logout` alla pagina di *Indice* :

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Si noti che la chiamata a `SignOutAsync` non specifica uno schema di autenticazione. Il `DefaultScheme` di `CookieAuthenticationDefaults.AuthenticationScheme` dell'app viene usato come fallback.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
