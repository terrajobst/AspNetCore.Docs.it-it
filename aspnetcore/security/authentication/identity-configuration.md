---
title: "Configurare l'identità di ASP.NET Core"
author: AdrienTorris
description: "Comprendere i valori predefiniti di ASP.NET Identity Core e configurare le varie proprietà di identità per l'utilizzo di valori personalizzati."
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 9e79e670173952f1e791a0cefba61c41e1ad4437
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="configure-identity"></a><span data-ttu-id="7901b-103">Configurare l'identità</span><span class="sxs-lookup"><span data-stu-id="7901b-103">Configure Identity</span></span>

<span data-ttu-id="7901b-104">ASP.NET Identity Core presenta comportamenti comuni nelle applicazioni, ad esempio criteri password, il periodo di blocco e le impostazioni dei cookie che è possibile sostituire facilmente l'applicazione `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="7901b-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="7901b-105">Criteri password</span><span class="sxs-lookup"><span data-stu-id="7901b-105">Passwords policy</span></span>

<span data-ttu-id="7901b-106">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="7901b-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="7901b-107">Esistono inoltre alcune altre restrizioni.</span><span class="sxs-lookup"><span data-stu-id="7901b-107">There are also some other restrictions.</span></span> <span data-ttu-id="7901b-108">Per semplificare le restrizioni di password, modificare il `ConfigureServices` metodo la `Startup` classe dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7901b-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7901b-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7901b-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7901b-110">Componenti di base di ASP.NET 2.0 è stato aggiunto il `RequiredUniqueChars` proprietà.</span><span class="sxs-lookup"><span data-stu-id="7901b-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="7901b-111">In caso contrario, le opzioni sono le stesse da ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="7901b-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7901b-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7901b-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="7901b-113">`IdentityOptions.Password`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="7901b-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="7901b-114">Proprietà</span><span class="sxs-lookup"><span data-stu-id="7901b-114">Property</span></span>                | <span data-ttu-id="7901b-115">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7901b-115">Description</span></span>                       | <span data-ttu-id="7901b-116">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7901b-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="7901b-117">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="7901b-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="7901b-118">true</span><span class="sxs-lookup"><span data-stu-id="7901b-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="7901b-119">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="7901b-119">The minimum length of the password.</span></span> | <span data-ttu-id="7901b-120">6</span><span class="sxs-lookup"><span data-stu-id="7901b-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="7901b-121">Richiede un carattere non alfanumerico nella password.</span><span class="sxs-lookup"><span data-stu-id="7901b-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="7901b-122">true</span><span class="sxs-lookup"><span data-stu-id="7901b-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="7901b-123">Richiede un carattere maiuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="7901b-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="7901b-124">true</span><span class="sxs-lookup"><span data-stu-id="7901b-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="7901b-125">Richiede un carattere minuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="7901b-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="7901b-126">true</span><span class="sxs-lookup"><span data-stu-id="7901b-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="7901b-127">Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="7901b-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="7901b-128">1</span><span class="sxs-lookup"><span data-stu-id="7901b-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="7901b-129">Blocco dell'utente</span><span class="sxs-lookup"><span data-stu-id="7901b-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="7901b-130">`IdentityOptions.Lockout`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="7901b-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="7901b-131">Proprietà</span><span class="sxs-lookup"><span data-stu-id="7901b-131">Property</span></span>                | <span data-ttu-id="7901b-132">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7901b-132">Description</span></span>                       | <span data-ttu-id="7901b-133">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7901b-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="7901b-134">La quantità di tempo un utente è bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="7901b-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="7901b-135">5 minuti</span><span class="sxs-lookup"><span data-stu-id="7901b-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="7901b-136">Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="7901b-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="7901b-137">5</span><span class="sxs-lookup"><span data-stu-id="7901b-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="7901b-138">Determina se un nuovo utente può essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="7901b-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="7901b-139">true</span><span class="sxs-lookup"><span data-stu-id="7901b-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="7901b-140">Impostazioni di accesso</span><span class="sxs-lookup"><span data-stu-id="7901b-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="7901b-141">`IdentityOptions.SignIn`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="7901b-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="7901b-142">Proprietà</span><span class="sxs-lookup"><span data-stu-id="7901b-142">Property</span></span>                | <span data-ttu-id="7901b-143">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7901b-143">Description</span></span>                       | <span data-ttu-id="7901b-144">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7901b-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="7901b-145">Richiede una conferma tramite posta elettronica di accedere.</span><span class="sxs-lookup"><span data-stu-id="7901b-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="7901b-146">False</span><span class="sxs-lookup"><span data-stu-id="7901b-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="7901b-147">Richiede un numero di telefono di conferma accedere.</span><span class="sxs-lookup"><span data-stu-id="7901b-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="7901b-148">False</span><span class="sxs-lookup"><span data-stu-id="7901b-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="7901b-149">Impostazioni di convalida utente</span><span class="sxs-lookup"><span data-stu-id="7901b-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="7901b-150">`IdentityOptions.User`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="7901b-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="7901b-151">Proprietà</span><span class="sxs-lookup"><span data-stu-id="7901b-151">Property</span></span>                | <span data-ttu-id="7901b-152">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7901b-152">Description</span></span>                       | <span data-ttu-id="7901b-153">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7901b-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="7901b-154">Richiede che ogni utente dispone di un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="7901b-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="7901b-155">False</span><span class="sxs-lookup"><span data-stu-id="7901b-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="7901b-156">Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="7901b-156">Allowed characters in the username.</span></span> | <span data-ttu-id="7901b-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="7901b-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="7901b-158">Impostazioni dei cookie dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7901b-158">Application's cookie settings</span></span>

<span data-ttu-id="7901b-159">Ad esempio i criteri password, tutte le impostazioni dei cookie dell'applicazione modificabili nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="7901b-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7901b-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7901b-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7901b-161">In `ConfigureServices` nel `Startup` (classe), è possibile configurare il cookie dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7901b-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7901b-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7901b-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="7901b-163">`CookieAuthenticationOptions`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="7901b-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="7901b-164">Proprietà</span><span class="sxs-lookup"><span data-stu-id="7901b-164">Property</span></span>                | <span data-ttu-id="7901b-165">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7901b-165">Description</span></span>                       | <span data-ttu-id="7901b-166">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="7901b-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="7901b-167">Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="7901b-167">The name of the cookie.</span></span>  | <span data-ttu-id="7901b-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="7901b-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="7901b-169">Se è true, il cookie non è accessibile da script lato client.</span><span class="sxs-lookup"><span data-stu-id="7901b-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="7901b-170">true</span><span class="sxs-lookup"><span data-stu-id="7901b-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="7901b-171">Determina quanto tempo memorizzata il ticket di autenticazione nel cookie resterà valido dal punto di cui che viene creato.</span><span class="sxs-lookup"><span data-stu-id="7901b-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="7901b-172">14 giorni</span><span class="sxs-lookup"><span data-stu-id="7901b-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="7901b-173">Quando un utente non autorizzato, essi verrà reindirizzati a questo percorso per l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="7901b-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="7901b-174">/Account/Login</span><span class="sxs-lookup"><span data-stu-id="7901b-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="7901b-175">Quando un utente è disconnesso, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="7901b-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="7901b-176">/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="7901b-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="7901b-177">Quando un utente ha esito negativo di un controllo dell'autorizzazione, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="7901b-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="7901b-178">Se è true, verrà generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza.</span><span class="sxs-lookup"><span data-stu-id="7901b-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="7901b-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="7901b-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="7901b-180">Determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un codice di stato Unauthorized 401 viene modificato in un reindirizzamento 302 nel percorso di accesso.</span><span class="sxs-lookup"><span data-stu-id="7901b-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="7901b-181">true</span><span class="sxs-lookup"><span data-stu-id="7901b-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="7901b-182">Questo è importante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="7901b-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="7901b-183">Il nome logico per uno schema di autenticazione specifico.</span><span class="sxs-lookup"><span data-stu-id="7901b-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="7901b-184">Questo flag è rilevante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="7901b-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="7901b-185">Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="7901b-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
