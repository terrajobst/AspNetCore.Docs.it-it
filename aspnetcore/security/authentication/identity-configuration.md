---
title: Configurare ASP.NET Core Identity
author: AdrienTorris
description: Informazioni sui valori predefiniti di ASP.NET Core Identity e su come configurare le proprietà di identità per usare valori personalizzati.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 0faab001b981c79f6afa16b2a8cf80c1ef141b11
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011300"
---
# <a name="configure-aspnet-core-identity"></a>Configurare ASP.NET Core Identity

ASP.NET Core Identity Usa valori predefiniti per le impostazioni, ad esempio criteri password, blocco e la configurazione di cookie. Queste impostazioni possono essere sostituite nel `Startup` classe.

## <a name="identity-options"></a>Opzioni di identità

Il [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe rappresenta le opzioni che possono essere usate per configurare il sistema di identità. `IdentityOptions` deve essere impostata **dopo aver** chiamata `AddIdentity` o `AddDefaultIdentity`.

### <a name="claims-identity"></a>Identità delle attestazioni

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) consente di specificare il [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con le proprietà visualizzate nella tabella seguente.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per un'attestazione di ruolo. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione di timestamp di sicurezza. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione dell'identificatore utente. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione del nome utente. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Blocco

Blocco viene impostato nel [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metodo:

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Il codice precedente si basa sul `Login` modello di identità. 

Le opzioni di blocco sono impostate `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Il codice precedente imposta la [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con i valori predefiniti.

Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio.

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) consente di specificare il [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Determina se un nuovo utente può essere bloccato. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Quanto tempo un utente viene bloccato quando si verifica un blocco. | 5 minuti |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Il numero di tentativi di accesso non riuscito fino a quando un utente è bloccato, se il blocco è abilitato. | 5 |

### <a name="password"></a>Password

Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, lettera minuscola, una cifra e un carattere non alfanumerico. Le password devono essere composta da almeno sei caratteri. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) possono essere impostate nella `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) consente di specificare il [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Richiede un numero compreso tra 0 e 9 nella password. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La lunghezza minima della password. | 6 |

::: moniker range=">= aspnetcore-2.0"

| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Si applica solo ad ASP.NET Core 2.0 o versione successiva.<br><br> Richiede il numero di caratteri distinti nella password. | 1 |

::: moniker-end

| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Richiede una lettera minuscola nella password. | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Richiede un carattere non alfanumerici nella password. | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Richiede una lettera maiuscola nella password. | `true` |

### <a name="sign-in"></a>Accedi

Il codice seguente imposta `SignIn` impostazioni (impostazione predefinita valori):

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) consente di specificare il [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Richiede un indirizzo di posta elettronica confermato per l'accesso. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Richiede un numero di telefono confermato per l'accesso. | `false` |

### <a name="tokens"></a>token

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) consente di specificare il [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con le proprietà visualizzate nella tabella.


|                                                        Proprietà                                                         |                                                                                      Descrizione                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Ottiene o imposta il `AuthenticatorTokenProvider` usato per convalidare gli accessi a due fattori con un autenticatore.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Ottiene o imposta il `ChangeEmailTokenProvider` usata per generare i token usati in messaggi di posta elettronica conferma Modifica messaggio di posta elettronica.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Ottiene o imposta il `ChangePhoneNumberTokenProvider` usata per generare i token usati quando si modificano i numeri di telefono.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Ottiene o imposta il provider di token utilizzato per generare i token utilizzati nei messaggi di posta elettronica conferma account.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Ottiene o imposta il [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usata per generare i token utilizzati nei messaggi di posta elettronica la reimpostazione della password. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Consente di creare un [Provider di Token utente](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la chiave usata come nome del provider.                 |

### <a name="user"></a>Utente

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) consente di specificare il [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con le proprietà visualizzate nella tabella.

| Proprietà | Descrizione | Impostazione predefinita |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caratteri consentiti nel nome utente. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Richiede che ogni utente un messaggio di posta elettronica univoco. | `false` |

### <a name="cookie-settings"></a>Impostazioni dei cookie

Configurare il cookie dell'app in `Startup.ConfigureServices`. [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) deve essere chiamato **dopo** chiamata `AddIdentity` o `AddDefaultIdentity`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).
