---
title: Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core
author: guardrex
description: Informazioni su come stabilire attestazioni aggiuntive e i token da provider esterni.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 49c323fab64bd4ea52dd1d8cf2e43a79d4d0d0dc
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327352"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un'app ASP.NET Core può stabilire altre attestazioni e token dai provider di autenticazione esterni, ad esempio Facebook, Google, Microsoft e Twitter. Ogni provider rivela varie informazioni relative agli utenti sulla piattaforma, ma il modello per la ricezione e trasformazione dei dati utente in attestazioni aggiuntive è la stessa.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

Decidere quali provider di autenticazione esterni per il supporto nell'app. Per ogni provider, registrare l'app e ottenere un ID client e segreto client. Per altre informazioni, vedere <xref:security/authentication/social/index>. Il [app di esempio](#sample-app-instructions) Usa le [provider di autenticazione Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Impostare l'ID client e segreto client

Il provider di autenticazione OAuth stabilisce una relazione di trust con un'app usando un ID client e segreto client. ID client e i valori del segreto client vengono creati per l'app dal provider di autenticazione esterna quando l'app viene registrata con il provider. Ogni provider esterno che usa l'app deve essere configurato in modo indipendente con ID client e segreto client del provider. Per altre informazioni, vedere gli argomenti di provider di autenticazione esterno che si applicano al proprio scenario:

* [Autenticazione Facebook](xref:security/authentication/facebook-logins)
* [Autenticazione Google](xref:security/authentication/google-logins)
* [Autenticazione Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticazione Twitter](xref:security/authentication/twitter-logins)
* [Altri provider di autenticazione](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

L'app di esempio configura il provider di autenticazione Google con un ID client e segreto client fornito da Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a>Definire l'ambito di autenticazione

Specificare l'elenco di autorizzazioni per recuperare dal provider, specificando il <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Gli ambiti di autenticazione per comuni provider esterni vengono visualizzati nella tabella seguente.

| Provider  | Ambito                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

L'app di esempio aggiunge Google `plus.login` ambito alla richiesta di accesso Google + autorizzazioni:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a>Eseguire il mapping di chiavi di dati utente e crea le attestazioni

Nelle opzioni del provider, specificare un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> per ogni chiave nei dati di utente del provider esterno JSON per l'identità dell'app per la lettura su Accedi. Per altre informazioni sui tipi di attestazione, vedere <xref:System.Security.Claims.ClaimTypes>.

L'app di esempio crea un <xref:System.Security.Claims.ClaimTypes.Gender> attestazione dal `gender` chiave nei dati utente Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

Nelle <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un' <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) viene eseguito l'accesso all'App con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Durante il processo di accesso di <xref:Microsoft.AspNetCore.Identity.UserManager%601> può archiviare un `ApplicationUser` attestazioni per i dati utente disponibili dal <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

Nell'app di esempio, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) stabilisce una <xref:System.Security.Claims.ClaimTypes.Gender> attestazioni per i signed in `ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a>Salvare il token di accesso

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Definisce se i token di accesso e di aggiornamento devono essere archiviati nel <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> un'autorizzazione ha esito positivo. `SaveTokens` è impostato su `false` per impostazione predefinita per ridurre le dimensioni del cookie di autenticazione finale.

L'app di esempio imposta il valore della `SaveTokens` al `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

Quando `OnPostConfirmationAsync` viene eseguito, archiviare il token di accesso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dal provider esterno nel `ApplicationUser`del `AuthenticationProperties`.

L'app di esempio consente di salvare il token di accesso in:

* `OnPostConfirmationAsync` &ndash; Viene eseguito per la registrazione di nuovi utenti.
* `OnGetCallbackAsync` &ndash; Viene eseguito quando un utente registrato in precedenza accede all'app.

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a>Come aggiungere token personalizzati aggiuntivi

Per illustrare come aggiungere un token personalizzato, che viene archiviato come parte del `SaveTokens`, l'app di esempio aggiunge un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con l'attuale <xref:System.DateTime> per un' [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) di `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>Istruzioni di app di esempio

L'app di esempio viene illustrato come:

* Ottenere il sesso dell'utente da Google e archiviare un'attestazione di sesso con il valore.
* Store il token di accesso di Google dell'utente `AuthenticationProperties`.

Per usare l'app di esempio:

1. Registrare l'app e ottenere un ID client valido e il segreto client per l'autenticazione Google. Per altre informazioni, vedere <xref:security/authentication/google-logins>.
1. Fornire l'ID client e segreto client per l'app nel <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> di `Startup.ConfigureServices`.
1. Eseguire l'app e richiedere la pagina My attestazioni. Quando l'utente non viene eseguito l'accesso, l'app reindirizza a Google. Accedi con Google. Google reindirizza l'utente all'App (`/Home/MyClaims`). L'utente viene autenticato e viene caricata la pagina My attestazioni. L'attestazione relativa al sesso è presente in **attestazioni utente** con il valore ottenuto da Google. Il token di accesso viene visualizzato nei **le proprietà di autenticazione**.

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
