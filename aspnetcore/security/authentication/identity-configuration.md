---
title: "Configurare l'identità di ASP.NET Core"
author: AdrienTorris
description: "Comprendere i valori predefiniti di ASP.NET Identity Core e configurare le varie proprietà di identità per l'utilizzo di valori personalizzati."
keywords: "Autenticazione ASP.NET Core, identità, sicurezza"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: adf577ae1e1c752c3b1a332ec94a7a7627a7a4b4
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity"></a>Configurare l'identità

Alcuni comportamenti predefiniti che è possibile sostituire facilmente l'applicazione dispone di ASP.NET Identity Core `Startup` classe.

## <a name="passwords-policy"></a>Criteri password

Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere alfanumerico. Esistono inoltre alcune altre restrizioni. Se si desidera semplificare restrizioni per le password, è possibile eseguire questa operazione nel `Startup` classe dell'applicazione.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Componenti di base di ASP.NET 2.0 è stato aggiunto il `RequiredUniqueChars` proprietà. In caso contrario, le opzioni sono le stesse da ASP.NET Core 1. x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`presenta le seguenti proprietà:
* `RequireDigit`: Richiede un numero compreso tra 0 e 9 nella password. True per impostazione predefinita.
* `RequiredLength`: La lunghezza minima della password. Valore predefinito è 6.
* `RequireNonAlphanumeric`: Richiede un carattere non alfanumerico nella password. True per impostazione predefinita.
* `RequireUppercase`: Richiede un carattere maiuscolo nella password. True per impostazione predefinita.
* `RequireLowercase`: Richiede un carattere minuscolo nella password. True per impostazione predefinita.
* `RequiredUniqueChars`: Richiede il numero di caratteri distinti nella password. Il valore predefinito è 1.


## <a name="users-lockout"></a>Blocco dell'utente

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`presenta le seguenti proprietà:
* `DefaultLockoutTimeSpan`: La quantità di tempo un utente è bloccato quando si verifica un blocco. Valore predefinito è 5 minuti.
* `MaxFailedAccessAttempts`: Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato. Valore predefinito è 5.
* `AllowedForNewUsers`: Determina se un nuovo utente può essere bloccato. True per impostazione predefinita.


## <a name="sign-in-settings"></a>Impostazioni di accesso

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`presenta le seguenti proprietà:
* `RequireConfirmedEmail`: Richiede una conferma tramite posta elettronica di accedere. Valore predefinito è false.
* `RequireConfirmedPhoneNumber`: Richiede un numero di telefono di conferma accedere. Valore predefinito è false.


## <a name="user-validation-settings"></a>Impostazioni di convalida utente

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`presenta le seguenti proprietà:
* `RequireUniqueEmail`: Richiede che ogni utente dispone di un messaggio di posta elettronica univoco. Valore predefinito è false.
* `AllowedUserNameCharacters`: Caratteri consentiti nel nome utente. Il valore predefinito abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Impostazioni dei cookie dell'applicazione

Ad esempio i criteri password, tutte le impostazioni dei cookie dell'applicazione modificabili nel `Startup` classe.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

In `ConfigureServices` nel `Startup` (classe), è possibile configurare il cookie dell'applicazione.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`presenta le seguenti proprietà:
* `Cookie.Name`: Il nome del cookie. Per impostazione predefinita. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Se è true, il cookie non è accessibile da script lato client. True per impostazione predefinita.
* `ExpireTimeSpan`: Determina quanto tempo memorizzata il ticket di autenticazione nel cookie resterà valido dal punto di cui che viene creato. Valore predefinito è 14 giorni.
* `LoginPath`: Quando un utente non autorizzato, essi verrà reindirizzati a questo percorso per l'account di accesso. Il valore predefinito a/Account/account di accesso.
* `LogoutPath`: Quando un utente è disconnesso, essi verrà reindirizzati a questo percorso. Il valore predefinito/Account/disconnessione.
* `AccessDeniedPath`: Quando un utente ha esito negativo di un controllo dell'autorizzazione, essi verrà reindirizzati a questo percorso. Il valore predefinito/Account/AccessDenied.
* `SlidingExpiration`: Se è true, verrà generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza. True per impostazione predefinita.
* `ReturnUrlParameter`: ReturnUrlParameter determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un codice di stato Unauthorized 401 viene modificato in un reindirizzamento 302 nel percorso di accesso.
* `AuthenticationScheme`: Questa opzione è pertinente solo per ASP.NET Core 1. x. Il nome logico per uno schema di autenticazione specifico.
* `AutomaticAuthenticate`: Questo flag è rilevante solo per ASP.NET Core 1. x. Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.

