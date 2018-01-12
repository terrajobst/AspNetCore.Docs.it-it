---
title: "Configurare l'identità di ASP.NET Core"
author: AdrienTorris
description: "Comprendere i valori predefiniti di ASP.NET Identity Core e configurare le varie proprietà di identità per l'utilizzo di valori personalizzati."
keywords: "Autenticazione ASP.NET Core, identità, sicurezza"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="d02b5-104">Configurare l'identità</span><span class="sxs-lookup"><span data-stu-id="d02b5-104">Configure Identity</span></span>

<span data-ttu-id="d02b5-105">ASP.NET Identity Core presenta comportamenti comuni nelle applicazioni, ad esempio criteri password, il periodo di blocco e le impostazioni dei cookie che è possibile sostituire facilmente l'applicazione `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="d02b5-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="d02b5-106">Criteri password</span><span class="sxs-lookup"><span data-stu-id="d02b5-106">Passwords policy</span></span>

<span data-ttu-id="d02b5-107">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="d02b5-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="d02b5-108">Esistono inoltre alcune altre restrizioni.</span><span class="sxs-lookup"><span data-stu-id="d02b5-108">There are also some other restrictions.</span></span> <span data-ttu-id="d02b5-109">Per semplificare le restrizioni di password, modificare il `ConfigureServices` metodo la `Startup` classe dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d02b5-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d02b5-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d02b5-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d02b5-111">Componenti di base di ASP.NET 2.0 è stato aggiunto il `RequiredUniqueChars` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d02b5-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="d02b5-112">In caso contrario, le opzioni sono le stesse da ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="d02b5-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d02b5-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d02b5-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="d02b5-114">`IdentityOptions.Password`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="d02b5-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="d02b5-115">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d02b5-115">Property</span></span>                | <span data-ttu-id="d02b5-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d02b5-116">Description</span></span>                       | <span data-ttu-id="d02b5-117">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d02b5-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="d02b5-118">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="d02b5-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="d02b5-119">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="d02b5-120">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="d02b5-120">The minimum length of the password.</span></span> | <span data-ttu-id="d02b5-121">6</span><span class="sxs-lookup"><span data-stu-id="d02b5-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="d02b5-122">Richiede un carattere non alfanumerico nella password.</span><span class="sxs-lookup"><span data-stu-id="d02b5-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="d02b5-123">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="d02b5-124">Richiede un carattere maiuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="d02b5-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="d02b5-125">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="d02b5-126">Richiede un carattere minuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="d02b5-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="d02b5-127">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="d02b5-128">Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="d02b5-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="d02b5-129">1</span><span class="sxs-lookup"><span data-stu-id="d02b5-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="d02b5-130">Blocco dell'utente</span><span class="sxs-lookup"><span data-stu-id="d02b5-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="d02b5-131">`IdentityOptions.Lockout`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="d02b5-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="d02b5-132">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d02b5-132">Property</span></span>                | <span data-ttu-id="d02b5-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d02b5-133">Description</span></span>                       | <span data-ttu-id="d02b5-134">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d02b5-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="d02b5-135">La quantità di tempo un utente è bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="d02b5-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="d02b5-136">5 minuti</span><span class="sxs-lookup"><span data-stu-id="d02b5-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="d02b5-137">Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="d02b5-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="d02b5-138">5</span><span class="sxs-lookup"><span data-stu-id="d02b5-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="d02b5-139">Determina se un nuovo utente può essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="d02b5-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="d02b5-140">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="d02b5-141">Impostazioni di accesso</span><span class="sxs-lookup"><span data-stu-id="d02b5-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="d02b5-142">`IdentityOptions.SignIn`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="d02b5-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="d02b5-143">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d02b5-143">Property</span></span>                | <span data-ttu-id="d02b5-144">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d02b5-144">Description</span></span>                       | <span data-ttu-id="d02b5-145">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d02b5-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="d02b5-146">Richiede una conferma tramite posta elettronica di accedere.</span><span class="sxs-lookup"><span data-stu-id="d02b5-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="d02b5-147">False</span><span class="sxs-lookup"><span data-stu-id="d02b5-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="d02b5-148">Richiede un numero di telefono di conferma accedere.</span><span class="sxs-lookup"><span data-stu-id="d02b5-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="d02b5-149">False</span><span class="sxs-lookup"><span data-stu-id="d02b5-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="d02b5-150">Impostazioni di convalida utente</span><span class="sxs-lookup"><span data-stu-id="d02b5-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="d02b5-151">`IdentityOptions.User`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="d02b5-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="d02b5-152">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d02b5-152">Property</span></span>                | <span data-ttu-id="d02b5-153">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d02b5-153">Description</span></span>                       | <span data-ttu-id="d02b5-154">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d02b5-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="d02b5-155">Richiede che ogni utente dispone di un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="d02b5-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="d02b5-156">False</span><span class="sxs-lookup"><span data-stu-id="d02b5-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="d02b5-157">Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="d02b5-157">Allowed characters in the username.</span></span> | <span data-ttu-id="d02b5-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="d02b5-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="d02b5-159">Impostazioni dei cookie dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d02b5-159">Application's cookie settings</span></span>

<span data-ttu-id="d02b5-160">Ad esempio i criteri password, tutte le impostazioni dei cookie dell'applicazione modificabili nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="d02b5-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d02b5-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d02b5-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d02b5-162">In `ConfigureServices` nel `Startup` (classe), è possibile configurare il cookie dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d02b5-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d02b5-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d02b5-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="d02b5-164">`CookieAuthenticationOptions`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="d02b5-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="d02b5-165">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d02b5-165">Property</span></span>                | <span data-ttu-id="d02b5-166">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d02b5-166">Description</span></span>                       | <span data-ttu-id="d02b5-167">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d02b5-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="d02b5-168">Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="d02b5-168">The name of the cookie.</span></span>  | <span data-ttu-id="d02b5-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="d02b5-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="d02b5-170">Se è true, il cookie non è accessibile da script lato client.</span><span class="sxs-lookup"><span data-stu-id="d02b5-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="d02b5-171">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="d02b5-172">Determina quanto tempo memorizzata il ticket di autenticazione nel cookie resterà valido dal punto di cui che viene creato.</span><span class="sxs-lookup"><span data-stu-id="d02b5-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="d02b5-173">14 giorni</span><span class="sxs-lookup"><span data-stu-id="d02b5-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="d02b5-174">Quando un utente non autorizzato, essi verrà reindirizzati a questo percorso per l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="d02b5-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="d02b5-175">/ / Accesso dell'account</span><span class="sxs-lookup"><span data-stu-id="d02b5-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="d02b5-176">Quando un utente è disconnesso, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="d02b5-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="d02b5-177">/ Account/disconnessione</span><span class="sxs-lookup"><span data-stu-id="d02b5-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="d02b5-178">Quando un utente ha esito negativo di un controllo dell'autorizzazione, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="d02b5-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="d02b5-179">Se è true, verrà generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza.</span><span class="sxs-lookup"><span data-stu-id="d02b5-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="d02b5-180">/ Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="d02b5-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="d02b5-181">Determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un codice di stato Unauthorized 401 viene modificato in un reindirizzamento 302 nel percorso di accesso.</span><span class="sxs-lookup"><span data-stu-id="d02b5-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="d02b5-182">true</span><span class="sxs-lookup"><span data-stu-id="d02b5-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="d02b5-183">Questo è importante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="d02b5-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="d02b5-184">Il nome logico per uno schema di autenticazione specifico.</span><span class="sxs-lookup"><span data-stu-id="d02b5-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="d02b5-185">Questo flag è rilevante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="d02b5-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="d02b5-186">Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="d02b5-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |