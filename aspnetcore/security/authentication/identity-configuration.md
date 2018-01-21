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
ms.openlocfilehash: d3a13d1cef3417522460b44c52c1361c3e9d1162
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="configure-identity"></a><span data-ttu-id="834e6-103">Configurare l'identità</span><span class="sxs-lookup"><span data-stu-id="834e6-103">Configure Identity</span></span>

<span data-ttu-id="834e6-104">ASP.NET Identity Core presenta comportamenti comuni nelle applicazioni, ad esempio criteri password, il periodo di blocco e le impostazioni dei cookie che è possibile sostituire facilmente l'applicazione `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="834e6-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="834e6-105">Criteri password</span><span class="sxs-lookup"><span data-stu-id="834e6-105">Passwords policy</span></span>

<span data-ttu-id="834e6-106">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, caratteri minuscoli in caratteri, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="834e6-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="834e6-107">Esistono inoltre alcune altre restrizioni.</span><span class="sxs-lookup"><span data-stu-id="834e6-107">There are also some other restrictions.</span></span> <span data-ttu-id="834e6-108">Per semplificare le restrizioni di password, modificare il `ConfigureServices` metodo la `Startup` classe dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="834e6-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="834e6-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="834e6-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="834e6-110">Componenti di base di ASP.NET 2.0 è stato aggiunto il `RequiredUniqueChars` proprietà.</span><span class="sxs-lookup"><span data-stu-id="834e6-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="834e6-111">In caso contrario, le opzioni sono le stesse da ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="834e6-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="834e6-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="834e6-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="834e6-113">`IdentityOptions.Password`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="834e6-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="834e6-114">Proprietà</span><span class="sxs-lookup"><span data-stu-id="834e6-114">Property</span></span>                | <span data-ttu-id="834e6-115">Descrizione</span><span class="sxs-lookup"><span data-stu-id="834e6-115">Description</span></span>                       | <span data-ttu-id="834e6-116">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="834e6-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="834e6-117">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="834e6-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="834e6-118">true</span><span class="sxs-lookup"><span data-stu-id="834e6-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="834e6-119">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="834e6-119">The minimum length of the password.</span></span> | <span data-ttu-id="834e6-120">6</span><span class="sxs-lookup"><span data-stu-id="834e6-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="834e6-121">Richiede un carattere non alfanumerico nella password.</span><span class="sxs-lookup"><span data-stu-id="834e6-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="834e6-122">true</span><span class="sxs-lookup"><span data-stu-id="834e6-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="834e6-123">Richiede un carattere maiuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="834e6-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="834e6-124">true</span><span class="sxs-lookup"><span data-stu-id="834e6-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="834e6-125">Richiede un carattere minuscolo nella password.</span><span class="sxs-lookup"><span data-stu-id="834e6-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="834e6-126">true</span><span class="sxs-lookup"><span data-stu-id="834e6-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="834e6-127">Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="834e6-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="834e6-128">1</span><span class="sxs-lookup"><span data-stu-id="834e6-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="834e6-129">Blocco dell'utente</span><span class="sxs-lookup"><span data-stu-id="834e6-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="834e6-130">`IdentityOptions.Lockout`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="834e6-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="834e6-131">Proprietà</span><span class="sxs-lookup"><span data-stu-id="834e6-131">Property</span></span>                | <span data-ttu-id="834e6-132">Descrizione</span><span class="sxs-lookup"><span data-stu-id="834e6-132">Description</span></span>                       | <span data-ttu-id="834e6-133">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="834e6-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="834e6-134">La quantità di tempo un utente è bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="834e6-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="834e6-135">5 minuti</span><span class="sxs-lookup"><span data-stu-id="834e6-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="834e6-136">Il numero di tentativi di accesso non riuscito fino a quando un utente viene bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="834e6-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="834e6-137">5</span><span class="sxs-lookup"><span data-stu-id="834e6-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="834e6-138">Determina se un nuovo utente può essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="834e6-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="834e6-139">true</span><span class="sxs-lookup"><span data-stu-id="834e6-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="834e6-140">Impostazioni di accesso</span><span class="sxs-lookup"><span data-stu-id="834e6-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="834e6-141">`IdentityOptions.SignIn`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="834e6-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="834e6-142">Proprietà</span><span class="sxs-lookup"><span data-stu-id="834e6-142">Property</span></span>                | <span data-ttu-id="834e6-143">Descrizione</span><span class="sxs-lookup"><span data-stu-id="834e6-143">Description</span></span>                       | <span data-ttu-id="834e6-144">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="834e6-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="834e6-145">Richiede una conferma tramite posta elettronica di accedere.</span><span class="sxs-lookup"><span data-stu-id="834e6-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="834e6-146">False</span><span class="sxs-lookup"><span data-stu-id="834e6-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="834e6-147">Richiede un numero di telefono di conferma accedere.</span><span class="sxs-lookup"><span data-stu-id="834e6-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="834e6-148">False</span><span class="sxs-lookup"><span data-stu-id="834e6-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="834e6-149">Impostazioni di convalida utente</span><span class="sxs-lookup"><span data-stu-id="834e6-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="834e6-150">`IdentityOptions.User`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="834e6-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="834e6-151">Proprietà</span><span class="sxs-lookup"><span data-stu-id="834e6-151">Property</span></span>                | <span data-ttu-id="834e6-152">Descrizione</span><span class="sxs-lookup"><span data-stu-id="834e6-152">Description</span></span>                       | <span data-ttu-id="834e6-153">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="834e6-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="834e6-154">Richiede che ogni utente dispone di un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="834e6-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="834e6-155">False</span><span class="sxs-lookup"><span data-stu-id="834e6-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="834e6-156">Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="834e6-156">Allowed characters in the username.</span></span> | <span data-ttu-id="834e6-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="834e6-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="834e6-158">Impostazioni dei cookie dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="834e6-158">Application's cookie settings</span></span>

<span data-ttu-id="834e6-159">Ad esempio i criteri password, tutte le impostazioni dei cookie dell'applicazione modificabili nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="834e6-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="834e6-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="834e6-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="834e6-161">In `ConfigureServices` nel `Startup` (classe), è possibile configurare il cookie dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="834e6-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="834e6-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="834e6-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="834e6-163">`CookieAuthenticationOptions`presenta le seguenti proprietà:</span><span class="sxs-lookup"><span data-stu-id="834e6-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="834e6-164">Proprietà</span><span class="sxs-lookup"><span data-stu-id="834e6-164">Property</span></span>                | <span data-ttu-id="834e6-165">Descrizione</span><span class="sxs-lookup"><span data-stu-id="834e6-165">Description</span></span>                       | <span data-ttu-id="834e6-166">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="834e6-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="834e6-167">Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="834e6-167">The name of the cookie.</span></span>  | <span data-ttu-id="834e6-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="834e6-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="834e6-169">Se è true, il cookie non è accessibile da script lato client.</span><span class="sxs-lookup"><span data-stu-id="834e6-169">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="834e6-170">true</span><span class="sxs-lookup"><span data-stu-id="834e6-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="834e6-171">Determina quanto tempo memorizzata il ticket di autenticazione nel cookie resterà valido dal punto di cui che viene creato.</span><span class="sxs-lookup"><span data-stu-id="834e6-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="834e6-172">14 giorni</span><span class="sxs-lookup"><span data-stu-id="834e6-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="834e6-173">Quando un utente non autorizzato, essi verrà reindirizzati a questo percorso per l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="834e6-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="834e6-174">/Account/Login</span><span class="sxs-lookup"><span data-stu-id="834e6-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="834e6-175">Quando un utente è disconnesso, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="834e6-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="834e6-176">/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="834e6-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="834e6-177">Quando un utente ha esito negativo di un controllo dell'autorizzazione, essi verrà reindirizzati a questo percorso.</span><span class="sxs-lookup"><span data-stu-id="834e6-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="834e6-178">Se è true, verrà generato un nuovo cookie con la nuova ora di scadenza quando il cookie corrente ha superato la metà nella finestra di scadenza.</span><span class="sxs-lookup"><span data-stu-id="834e6-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="834e6-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="834e6-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="834e6-180">Determina il nome del parametro della stringa di query che viene aggiunto dal middleware quando un codice di stato Unauthorized 401 viene modificato in un reindirizzamento 302 nel percorso di accesso.</span><span class="sxs-lookup"><span data-stu-id="834e6-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="834e6-181">true</span><span class="sxs-lookup"><span data-stu-id="834e6-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="834e6-182">Questo è importante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="834e6-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="834e6-183">Il nome logico per uno schema di autenticazione specifico.</span><span class="sxs-lookup"><span data-stu-id="834e6-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="834e6-184">Questo flag è rilevante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="834e6-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="834e6-185">Se è true, all'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="834e6-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
