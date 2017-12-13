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
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="88382-104">Configurare l'identità</span><span class="sxs-lookup"><span data-stu-id="88382-104">Configure Identity</span></span>

<span data-ttu-id="88382-105">Alcuni comportamenti predefiniti che è possibile sostituire facilmente l'applicazione dispone di ASP.NET Identity Core `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="88382-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="88382-106">Criteri password</span><span class="sxs-lookup"><span data-stu-id="88382-106">Passwords policy</span></span>

<span data-ttu-id="88382-107">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="88382-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="88382-108">Esistono inoltre alcune altre restrizioni.</span><span class="sxs-lookup"><span data-stu-id="88382-108">There are also some other restrictions.</span></span> <span data-ttu-id="88382-109">Se si desidera semplificare restrizioni per le password, è possibile eseguire questa operazione nel `Startup` classe dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="88382-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88382-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88382-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="88382-111">Componenti di base di ASP.NET 2.0 è stato aggiunto il `RequiredUniqueChars` proprietà.</span><span class="sxs-lookup"><span data-stu-id="88382-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="88382-112">In caso contrario, le opzioni sono le stesse da ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="88382-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88382-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88382-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="88382-114">`IdentityOptions.Password`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="88382-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="88382-115">`RequireDigit`: Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="88382-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="88382-116">True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-116">Defaults to true.</span></span>
* <span data-ttu-id="88382-117">`RequiredLength`: La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="88382-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="88382-118">Valore predefinito è 6.</span><span class="sxs-lookup"><span data-stu-id="88382-118">Defaults to 6.</span></span>
* <span data-ttu-id="88382-119">`RequireNonAlphanumeric`: Richiede un carattere non alfanumerico nella password.</span><span class="sxs-lookup"><span data-stu-id="88382-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="88382-120">True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-120">Defaults to true.</span></span>
* <span data-ttu-id="88382-121">`RequireUppercase`: Richiede un carattere maiuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="88382-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="88382-122">True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-122">Defaults to true.</span></span>
* <span data-ttu-id="88382-123">`RequireLowercase`: Richiede un carattere minuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="88382-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="88382-124">True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-124">Defaults to true.</span></span>
* <span data-ttu-id="88382-125">`RequiredUniqueChars`: Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="88382-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="88382-126">Il valore predefinito è 1.</span><span class="sxs-lookup"><span data-stu-id="88382-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="88382-127">Blocco dell'utente</span><span class="sxs-lookup"><span data-stu-id="88382-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="88382-128">`IdentityOptions.Lockout`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="88382-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="88382-129">`DefaultLockoutTimeSpan`: La quantità di tempo un utente è bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="88382-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="88382-130">Valore predefinito è 5 minuti.</span><span class="sxs-lookup"><span data-stu-id="88382-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="88382-131">`MaxFailedAccessAttempts`: Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="88382-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="88382-132">Valore predefinito è 5.</span><span class="sxs-lookup"><span data-stu-id="88382-132">Defaults to 5.</span></span>
* <span data-ttu-id="88382-133">`AllowedForNewUsers`: Determina se un nuovo utente può essere bloccato. True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="88382-134">Impostazioni di accesso</span><span class="sxs-lookup"><span data-stu-id="88382-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="88382-135">`IdentityOptions.SignIn`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="88382-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="88382-136">`RequireConfirmedEmail`: Richiede una conferma tramite posta elettronica di accedere.</span><span class="sxs-lookup"><span data-stu-id="88382-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="88382-137">Valore predefinito è false.</span><span class="sxs-lookup"><span data-stu-id="88382-137">Defaults to false.</span></span>
* <span data-ttu-id="88382-138">`RequireConfirmedPhoneNumber`: Richiede un numero di telefono di conferma accedere.</span><span class="sxs-lookup"><span data-stu-id="88382-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="88382-139">Valore predefinito è false.</span><span class="sxs-lookup"><span data-stu-id="88382-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="88382-140">Impostazioni di convalida utente</span><span class="sxs-lookup"><span data-stu-id="88382-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="88382-141">`IdentityOptions.User`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="88382-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="88382-142">`RequireUniqueEmail`: Richiede che ogni utente dispone di un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="88382-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="88382-143">Valore predefinito è false.</span><span class="sxs-lookup"><span data-stu-id="88382-143">Defaults to false.</span></span>
* <span data-ttu-id="88382-144">`AllowedUserNameCharacters`: Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="88382-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="88382-145">Il valore predefinito abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="88382-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="88382-146">Impostazioni dei cookie dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="88382-146">Application's cookie settings</span></span>

<span data-ttu-id="88382-147">Ad esempio i criteri password, tutte le impostazioni dei cookie dell'applicazione modificabili nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="88382-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88382-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88382-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="88382-149">In `ConfigureServices` nel `Startup` (classe), è possibile configurare il cookie dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="88382-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88382-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88382-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="88382-151">`CookieAuthenticationOptions`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="88382-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="88382-152">`Cookie.Name`: Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="88382-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="88382-153">Per impostazione predefinita. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="88382-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="88382-154">`Cookie.HttpOnly`: Se è true, il cookie non è accessibile da script lato client.</span><span class="sxs-lookup"><span data-stu-id="88382-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="88382-155">True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-155">Defaults to true.</span></span>
* <span data-ttu-id="88382-156">`ExpireTimeSpan`: Determina quanto tempo memorizzata il ticket di autenticazione nel cookie resterà valido dal punto di cui che viene creato.</span><span class="sxs-lookup"><span data-stu-id="88382-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="88382-157">Valore predefinito è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="88382-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="88382-158">`LoginPath`: Quando un utente non autorizzato, essi verrà reindirizzati a questo percorso per l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="88382-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="88382-159">Il valore predefinito a/Account/account di accesso.</span><span class="sxs-lookup"><span data-stu-id="88382-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="88382-160">`LogoutPath`: Quando un utente è disconnesso, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="88382-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="88382-161">Il valore predefinito/Account/disconnessione.</span><span class="sxs-lookup"><span data-stu-id="88382-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="88382-162">`AccessDeniedPath`: Quando un utente ha esito negativo di un controllo dell'autorizzazione, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="88382-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="88382-163">Il valore predefinito/Account/AccessDenied.</span><span class="sxs-lookup"><span data-stu-id="88382-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="88382-164">`SlidingExpiration`: Se è true, verrà generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza.</span><span class="sxs-lookup"><span data-stu-id="88382-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="88382-165">True per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="88382-165">Defaults to true.</span></span>
* <span data-ttu-id="88382-166">`ReturnUrlParameter`: ReturnUrlParameter determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un codice di stato Unauthorized 401 viene modificato in un reindirizzamento 302 nel percorso di accesso.</span><span class="sxs-lookup"><span data-stu-id="88382-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="88382-167">`AuthenticationScheme`: Questa opzione è pertinente solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="88382-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="88382-168">Il nome logico per uno schema di autenticazione specifico.</span><span class="sxs-lookup"><span data-stu-id="88382-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="88382-169">`AutomaticAuthenticate`: Questo flag è rilevante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="88382-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="88382-170">Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="88382-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

