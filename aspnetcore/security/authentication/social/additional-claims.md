---
title: Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core
author: guardrex
description: Informazioni su come stabilire attestazioni aggiuntive e i token da provider esterni.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874850"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un'app ASP.NET Core può stabilire altre attestazioni e token dai provider di autenticazione esterni, ad esempio Facebook, Google, Microsoft e Twitter. Ogni provider rivela varie informazioni relative agli utenti sulla piattaforma, ma il modello per la ricezione e trasformazione dei dati utente in attestazioni aggiuntive è la stessa.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

Decidere quali provider di autenticazione esterni per il supporto nell'app. Per ogni provider, registrare l'app e ottenere un ID client e segreto client. Per altre informazioni, vedere <xref:security/authentication/social/index>. L'app di esempio Usa la [provider di autenticazione Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Impostare l'ID client e segreto client

Il provider di autenticazione OAuth stabilisce una relazione di trust con un'app usando un ID client e segreto client. ID client e i valori del segreto client vengono creati per l'app dal provider di autenticazione esterna quando l'app viene registrata con il provider. Ogni provider esterno che usa l'app deve essere configurato in modo indipendente con ID client e segreto client del provider. Per altre informazioni, vedere gli argomenti di provider di autenticazione esterno che si applicano al proprio scenario:

* [Autenticazione Facebook](xref:security/authentication/facebook-logins)
* [Autenticazione Google](xref:security/authentication/google-logins)
* [Autenticazione Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticazione Twitter](xref:security/authentication/twitter-logins)
* [Altri provider di autenticazione](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

L'app di esempio configura il provider di autenticazione Google con un ID client e segreto client fornito da Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Definire l'ambito di autenticazione

Specificare l'elenco di autorizzazioni per recuperare dal provider, specificando il <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Gli ambiti di autenticazione per comuni provider esterni vengono visualizzati nella tabella seguente.

| Provider  | Ambito                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Nell'app di esempio, Google `userinfo.profile` ambito viene aggiunto automaticamente dal framework quando <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> viene chiamato sul <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Se l'app richiede altri ambiti, aggiungerli alle opzioni. Nell'esempio seguente, Google `https://www.googleapis.com/auth/user.birthday.read` ambito viene aggiunto per poter recuperare data di nascita dell'utente:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Eseguire il mapping di chiavi di dati utente e crea le attestazioni

Nelle opzioni del provider, specificare una <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> per ogni chiave/sottochiave nei dati di utente del provider esterno JSON per l'identità dell'app per la lettura su Accedi. Per altre informazioni sui tipi di attestazione, vedere <xref:System.Security.Claims.ClaimTypes>.

L'app di esempio crea le impostazioni locali (`urn:google:locale`) e l'immagine (`urn:google:picture`) di attestazioni dal `locale` e `picture` chiavi nei dati utente Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

Nelle <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un' <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) viene eseguito l'accesso all'App con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Durante il processo di accesso di <xref:Microsoft.AspNetCore.Identity.UserManager%601> può archiviare un `ApplicationUser` attestazioni per i dati utente disponibile dal <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

Nell'app di esempio, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) stabilisce le impostazioni locali (`urn:google:locale`) e l'immagine (`urn:google:picture`) le attestazioni per i signed in `ApplicationUser`, tra cui un'attestazione per <xref:System.Security.Claims.ClaimTypes.GivenName> :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

Per impostazione predefinita, le attestazioni dell'utente vengono archiviate nel cookie di autenticazione. Se il cookie di autenticazione è troppo grande, è possibile che l'app a non riuscire perché:

* Il browser rileva che l'intestazione del cookie è troppo lungo.
* La dimensione complessiva della richiesta è troppo grande.

Se una grande quantità di dati dell'utente è obbligatorio per l'elaborazione delle richieste utente:

* Limitare il numero e dimensione delle attestazioni utente per a ciò che l'app richiede solo di elaborazione della richiesta.
* Usare una classe personalizzata <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> per il Middleware di autenticazione Cookie <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> per archiviare l'identità tra le richieste. Mantenere grandi quantità di informazioni sull'identità nel server durante l'invio solo di una chiave di identificatore di sessione di piccole dimensioni al client.

## <a name="save-the-access-token"></a>Salvare il token di accesso

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Definisce se i token di accesso e di aggiornamento devono essere archiviati nel <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> un'autorizzazione ha esito positivo. `SaveTokens` è impostato su `false` per impostazione predefinita per ridurre le dimensioni del cookie di autenticazione finale.

L'app di esempio imposta il valore della `SaveTokens` al `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

Quando `OnPostConfirmationAsync` viene eseguito, archiviare il token di accesso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dal provider esterno nel `ApplicationUser`del `AuthenticationProperties`.

L'app di esempio consente di salvare il token di accesso nel `OnPostConfirmationAsync` (registrazione di nuovi utenti) e `OnGetCallbackAsync` (utente registrato in precedenza) nella *Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Come aggiungere token personalizzati aggiuntivi

Per illustrare come aggiungere un token personalizzato, che viene archiviato come parte del `SaveTokens`, l'app di esempio aggiunge un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con l'attuale <xref:System.DateTime> per un' [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) di `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>Creazione e aggiunta di attestazioni

Il framework fornisce azioni comuni e i metodi di estensione per la creazione e aggiunta di attestazioni all'insieme. Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> e <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Gli utenti possono definire le azioni personalizzate derivandole dalla <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e l'implementazione astratta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> (metodo).

Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Rimozione di azioni di attestazione e attestazioni

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) rimuove tutte le attestazioni azioni per il dato <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> dalla raccolta. [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) consente di eliminare un'attestazione di dato <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> dall'identità. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> viene usato principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) rimuovere attestazioni generati dal protocollo.

## <a name="sample-app-output"></a>Output di app di esempio

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [app SocialSample engineering ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l'app di esempio collegata è nel [del repository di GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` ramo engineering. Il `master` ramo contiene il codice in fase di sviluppo della prossima versione di ASP.NET Core. Per visualizzare una versione dell'app di esempio per una versione rilasciata di ASP.NET Core, usare il **ramo** menu a discesa per selezionare un ramo di rilascio (ad esempio `release/2.2`).
