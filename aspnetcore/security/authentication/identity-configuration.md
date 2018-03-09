---
title: "Configurare l'identità di ASP.NET Core"
author: AdrienTorris
description: "Informazioni sui valori predefiniti di ASP.NET Identity Core e informazioni su come configurare le proprietà di identità per l'utilizzo di valori personalizzati."
manager: wpickett
ms.author: scaddie
ms.date: 03/06/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 469068af2fc12627a0a5d1c5623eb60bef51cea0
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2018
---
# <a name="configure-identity"></a>Configurare l'identità

Identità di ASP.NET Core utilizza configurazione predefinita per le impostazioni come criteri password, il periodo di blocco e le impostazioni dei cookie. Queste impostazioni possono essere sostituite dell'app `Startup` classe.

## <a name="identity-options"></a>Opzioni di identità

Il [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe rappresenta le opzioni che possono essere usate per configurare il sistema di identità.

### <a name="claims-identity"></a>Identità basata sulle attestazioni

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifica il [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per un'attestazione di ruolo. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione di indicatore di sicurezza. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione dell'identificatore utente. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione del nome utente. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Blocco

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifica il [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Determina se un nuovo utente può essere bloccato. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | La quantità di tempo un utente è bloccato quando si verifica un blocco. | 5 minuti |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato. | 5 |

### <a name="password"></a>Password

Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere non alfanumerico. Le password devono essere composta da almeno sei caratteri. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) possono essere modificati in `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Componenti di base di ASP.NET 2.0 è stato aggiunto il [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) proprietà. In caso contrario, le opzioni sono le stesse ASP.NET Core 1. x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifica il [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Richiede un numero compreso tra 0 e 9 nella password. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La lunghezza minima della password. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Si applica solo a componenti di base di ASP.NET 2.0 o versione successiva.<br><br> Richiede il numero di caratteri distinti nella password. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Richiede un carattere minuscolo nella password. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Richiede un carattere non alfanumerico nella password. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Richiede un carattere maiuscolo nella password. | `true` |

### <a name="sign-in"></a>Accedi

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifica il [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Richiede una conferma tramite posta elettronica di accedere. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Richiede un numero di telefono di conferma accedere. | `false` |

### <a name="tokens"></a>token

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifica il [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Ottiene o imposta il `AuthenticatorTokenProvider` utilizzato per convalidare gli accessi a due fattori con un autenticatore. |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Ottiene o imposta il `ChangeEmailTokenProvider` utilizzato per generare i token utilizzati nei messaggi e-mail di conferma Modifica messaggio di posta elettronica. |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Ottiene o imposta il `ChangePhoneNumberTokenProvider` utilizzato per generare i token utilizzati quando si modificano i numeri di telefono. |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Ottiene o imposta il provider di token usato per generare i token utilizzati nei messaggi e-mail di conferma di account. |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Ottiene o imposta il [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) utilizzato per generare i token utilizzati nei messaggi e-mail di reimpostazione della password. |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Utilizzato per costruire un [Provider di Token utente](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la chiave utilizzata come il nome del provider. |

### <a name="user"></a>Utente

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifica il [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caratteri consentiti nel nome utente. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Richiede che ogni utente dispone di un messaggio di posta elettronica univoco. | `false` |

## <a name="cookie-settings"></a>Impostazioni dei cookie

Configurare il cookie dell'applicazione in `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) presenta le seguenti proprietà:

| Proprietà | Descrizione |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | Informa il gestore che è necessario cambiare in uscita *403-accesso negato* codice di stato in un *reindirizzamento 302* nel percorso specificato.<br><br>Il valore predefinito è `/Account/AccessDenied`. |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | Si applica solo a ASP.NET di base 1. x.<br><br> Il nome logico per uno schema di autenticazione specifico. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | Si applica solo a ASP.NET di base 1. x.<br><br> Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | Si applica solo a ASP.NET di base 1. x.<br><br> Se true, il middleware di autenticazione gestisce sfide automatico. Se false, il middleware di autenticazione altera solo le risposte quando richiesto esplicitamente dal `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | Ottiene o imposta l'autorità emittente che deve essere utilizzato per tutte le attestazioni che vengono create (ereditata da [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Il dominio a cui associare il cookie. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | Ottiene o imposta l'intervallo di validità del cookie HTTP (non il cookie di autenticazione). Questa proprietà viene sostituita da [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan). E non deve essere utilizzato nel contesto di CookieAuthentication. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Indica se un cookie è accessibile da uno script sul lato client.<br><br>Il valore predefinito è `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Il nome del cookie.<br><br>Il valore predefinito è `.AspNetCore.Cookies`. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Il percorso del cookie. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | Il `SameSite` attributo del cookie.<br><br>Il valore predefinito è [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | Il [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) configurazione.<br><br>Il valore predefinito è [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy). |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | Si applica solo a ASP.NET di base 1. x.<br><br> Il nome di dominio in cui il cookie viene servito. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | Si applica solo a ASP.NET di base 1. x.<br><br> Flag che indica se il cookie deve essere accessibile solo ai server.<br><br>Il valore predefinito è `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | Si applica solo a ASP.NET di base 1. x.<br><br> Utilizzato per isolare le app eseguono lo stesso nome host. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | Si applica solo a ASP.NET di base 1. x.<br><br> Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`).<br><br>Il valore predefinito è `CookieSecurePolicy.SameAsRequest`. |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | Il componente utilizzato per ottenere i cookie dalla richiesta o impostarli sulla risposta. | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | Se è impostato, il provider utilizzato per il [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) per la protezione dati. |
| [Descrizione](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | Si applica solo a ASP.NET di base 1. x.<br><br> Informazioni aggiuntive sul tipo di autenticazione viene resa disponibile per l'app. |
| [Eventi](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | Il gestore chiama i metodi per il provider che offrono il controllo di app in determinati momenti in cui è in corso l'elaborazione. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | Se impostato, il servizio di tipo per ottenere il `Events` istanza anziché la proprietà (ereditata da [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | Controlla la quantità di tempo archiviato in rimangono i cookie validi dal punto che viene creato il ticket di autenticazione.<br><br>Il valore predefinito è 14 giorni. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | Quando un utente non autorizzato, viene indirizzato a questo percorso per l'account di accesso.<br><br>Il valore predefinito è `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | Quando un utente è disconnesso, viene indirizzato a questo percorso.<br><br>Il valore predefinito è `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | Determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un *401 non autorizzato* codice di stato viene modificato in un *reindirizzamento 302* nel percorso di accesso.<br><br>Il valore predefinito è `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | Un contenitore facoltativo in cui archiviare l'identità in tutte le richieste. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | Se è true, viene generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza.<br><br>Il valore predefinito è `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | Il `TicketDataFormat` viene utilizzata per proteggere e annullare la protezione dell'identità e altre proprietà che vengono archiviate nel valore del cookie. |
