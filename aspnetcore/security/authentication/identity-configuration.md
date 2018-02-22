---
title: "Configurare l'identità di ASP.NET Core"
author: AdrienTorris
description: "Comprendere i valori predefiniti di ASP.NET Identity Core e configurare le varie proprietà di identità per l'utilizzo di valori personalizzati."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 0ec223ce06ff116c36182b8de507138e96a277a4
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2018
---
# <a name="configure-identity"></a>Configurare l'identità

ASP.NET Identity Core presenta comportamenti comuni nelle applicazioni, ad esempio criteri password, il periodo di blocco e le impostazioni dei cookie che è possibile sostituire facilmente l'applicazione `Startup` classe.

## <a name="passwords-policy"></a>Criteri password

Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere non alfanumerico. Esistono inoltre alcune altre restrizioni. Per semplificare le restrizioni di password, modificare il `ConfigureServices` metodo la `Startup` classe dell'applicazione.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Componenti di base di ASP.NET 2.0 è stato aggiunto il `RequiredUniqueChars` proprietà. In caso contrario, le opzioni sono le stesse da ASP.NET Core 1. x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password` presenta le seguenti proprietà:

| Proprietà                | Descrizione                       | Impostazione predefinita |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Richiede un numero compreso tra 0 e 9 nella password. | true |
| `RequiredLength`        | La lunghezza minima della password. | 6 |
| `RequireNonAlphanumeric`| Richiede un carattere non alfanumerico nella password. | true |
| `RequireUppercase`      | Richiede un carattere maiuscolo nella password. | true |
| `RequireLowercase`      | Richiede un carattere minuscolo nella password. | true |
| `RequiredUniqueChars`   | Richiede il numero di caratteri distinti nella password. | 1 |


## <a name="users-lockout"></a>Blocco dell'utente

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout` presenta le seguenti proprietà:

| Proprietà                | Descrizione                       | Impostazione predefinita |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | La quantità di tempo un utente è bloccato quando si verifica un blocco.  | 5 minuti  |
| `MaxFailedAccessAttempts` | Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato.  | 5 |
| `AllowedForNewUsers` | Determina se un nuovo utente può essere bloccato.  | true |

## <a name="sign-in-settings"></a>Impostazioni di accesso

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn` presenta le seguenti proprietà:

| Proprietà                | Descrizione                       | Impostazione predefinita |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Richiede una conferma tramite posta elettronica di accedere. | False  |
| `RequireConfirmedPhoneNumber` |  Richiede un numero di telefono di conferma accedere. | False  |

## <a name="user-validation-settings"></a>Impostazioni di convalida utente

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User` presenta le seguenti proprietà:

| Proprietà                | Descrizione                       | Impostazione predefinita |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Richiede che ogni utente dispone di un messaggio di posta elettronica univoco. | False  |
| `AllowedUserNameCharacters`  | Caratteri consentiti nel nome utente. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Impostazioni dei cookie dell'applicazione

Ad esempio i criteri password, tutte le impostazioni dei cookie dell'applicazione modificabili nel `Startup` classe.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

In `ConfigureServices` nel `Startup` (classe), è possibile configurare il cookie dell'applicazione.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions` presenta le seguenti proprietà:

| Proprietà                | Descrizione                       | Impostazione predefinita |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Il nome del cookie.  | .AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | Se è true, il cookie non è accessibile da script lato client.  |  true |
| `ExpireTimeSpan`  | Determina quanto tempo memorizzata il ticket di autenticazione nel cookie resterà valido dal punto di cui che viene creato.  | 14 giorni  |
| `LoginPath`  | Quando un utente non autorizzato, essi verrà reindirizzati a questo percorso per l'account di accesso. | /Account/Login  |
| `LogoutPath`  | Quando un utente è disconnesso, essi verrà reindirizzati a questo percorso.  | /Account/Logout  |
| `AccessDeniedPath`  | Quando un utente ha esito negativo di un controllo dell'autorizzazione, essi verrà reindirizzati a questo percorso.  |  /Account/AccessDenied |
| `SlidingExpiration`  | Se è true, verrà generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza.  | true |
| `ReturnUrlParameter`  | Determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un codice di stato Unauthorized 401 viene modificato in un reindirizzamento 302 nel percorso di accesso.  | ReturnUrl |
| `AuthenticationScheme`  | Questo è importante solo per ASP.NET Core 1. x. Il nome logico per uno schema di autenticazione specifico. |  |
| `AutomaticAuthenticate`  | Questo flag è rilevante solo per ASP.NET Core 1. x. Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.  |  |
