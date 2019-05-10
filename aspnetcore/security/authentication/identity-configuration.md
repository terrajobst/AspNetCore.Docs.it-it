---
title: Configurare ASP.NET Core Identity
author: AdrienTorris
description: Informazioni sui valori predefiniti di ASP.NET Core Identity e su come configurare le proprietà di identità per usare valori personalizzati.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892348"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="0f7e4-103">Configurare ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="0f7e4-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="0f7e4-104">ASP.NET Core Identity Usa valori predefiniti per le impostazioni, ad esempio criteri password, blocco e la configurazione di cookie.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="0f7e4-105">Queste impostazioni possono essere sostituite nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="0f7e4-106">Opzioni di identità</span><span class="sxs-lookup"><span data-stu-id="0f7e4-106">Identity options</span></span>

<span data-ttu-id="0f7e4-107">Il [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe rappresenta le opzioni che possono essere usate per configurare il sistema di identità.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="0f7e4-108">`IdentityOptions` deve essere impostata **dopo aver** chiamata `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="0f7e4-109">Identità delle attestazioni</span><span class="sxs-lookup"><span data-stu-id="0f7e4-109">Claims Identity</span></span>

<span data-ttu-id="0f7e4-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) consente di specificare il [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con le proprietà visualizzate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="0f7e4-111">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-111">Property</span></span> | <span data-ttu-id="0f7e4-112">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-112">Description</span></span> | <span data-ttu-id="0f7e4-113">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="0f7e4-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0f7e4-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="0f7e4-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="0f7e4-115">Ottiene o imposta il tipo di attestazione utilizzato per un'attestazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="0f7e4-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="0f7e4-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="0f7e4-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="0f7e4-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="0f7e4-118">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione di timestamp di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="0f7e4-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="0f7e4-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="0f7e4-120">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione dell'identificatore utente.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="0f7e4-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="0f7e4-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="0f7e4-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="0f7e4-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="0f7e4-123">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione del nome utente.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="0f7e4-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="0f7e4-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="0f7e4-125">Blocco</span><span class="sxs-lookup"><span data-stu-id="0f7e4-125">Lockout</span></span>

<span data-ttu-id="0f7e4-126">Blocco viene impostato nel [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metodo:</span><span class="sxs-lookup"><span data-stu-id="0f7e4-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="0f7e4-127">Il codice precedente si basa sul `Login` modello di identità.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="0f7e4-128">Le opzioni di blocco sono impostate `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0f7e4-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="0f7e4-129">Il codice precedente imposta la [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="0f7e4-130">Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="0f7e4-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) consente di specificare il [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0f7e4-132">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-132">Property</span></span> | <span data-ttu-id="0f7e4-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-133">Description</span></span> | <span data-ttu-id="0f7e4-134">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="0f7e4-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0f7e4-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="0f7e4-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="0f7e4-136">Determina se un nuovo utente può essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="0f7e4-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="0f7e4-138">Quanto tempo un utente viene bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="0f7e4-139">5 minuti</span><span class="sxs-lookup"><span data-stu-id="0f7e4-139">5 minutes</span></span> |
| [<span data-ttu-id="0f7e4-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="0f7e4-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="0f7e4-141">Il numero di tentativi di accesso non riuscito fino a quando un utente è bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="0f7e4-142">5</span><span class="sxs-lookup"><span data-stu-id="0f7e4-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="0f7e4-143">Password</span><span class="sxs-lookup"><span data-stu-id="0f7e4-143">Password</span></span>

<span data-ttu-id="0f7e4-144">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, lettera minuscola, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="0f7e4-145">Le password devono essere composta da almeno sei caratteri.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="0f7e4-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) possono essere impostate nella `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="0f7e4-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) consente di specificare il [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="0f7e4-148">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-148">Property</span></span> | <span data-ttu-id="0f7e4-149">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-149">Description</span></span> | <span data-ttu-id="0f7e4-150">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="0f7e4-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0f7e4-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="0f7e4-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="0f7e4-152">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="0f7e4-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="0f7e4-154">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-154">The minimum length of the password.</span></span> | <span data-ttu-id="0f7e4-155">6</span><span class="sxs-lookup"><span data-stu-id="0f7e4-155">6</span></span> |
| [<span data-ttu-id="0f7e4-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="0f7e4-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="0f7e4-157">Richiede un carattere minuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="0f7e4-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="0f7e4-159">Richiede un carattere non alfanumerico della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="0f7e4-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="0f7e4-161">Si applica solo a ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="0f7e4-162">Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="0f7e4-163">1</span><span class="sxs-lookup"><span data-stu-id="0f7e4-163">1</span></span> |
| [<span data-ttu-id="0f7e4-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="0f7e4-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="0f7e4-165">Richiede un carattere maiuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="0f7e4-166">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-166">Property</span></span> | <span data-ttu-id="0f7e4-167">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-167">Description</span></span> | <span data-ttu-id="0f7e4-168">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="0f7e4-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0f7e4-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="0f7e4-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="0f7e4-170">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="0f7e4-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="0f7e4-172">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-172">The minimum length of the password.</span></span> | <span data-ttu-id="0f7e4-173">6</span><span class="sxs-lookup"><span data-stu-id="0f7e4-173">6</span></span> |
| [<span data-ttu-id="0f7e4-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="0f7e4-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="0f7e4-175">Richiede un carattere minuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="0f7e4-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="0f7e4-177">Richiede un carattere non alfanumerico della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="0f7e4-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="0f7e4-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="0f7e4-179">Richiede un carattere maiuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="0f7e4-180">Accesso</span><span class="sxs-lookup"><span data-stu-id="0f7e4-180">Sign-in</span></span>

<span data-ttu-id="0f7e4-181">Il codice seguente imposta `SignIn` impostazioni (impostazione predefinita valori):</span><span class="sxs-lookup"><span data-stu-id="0f7e4-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="0f7e4-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) consente di specificare il [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0f7e4-183">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-183">Property</span></span> | <span data-ttu-id="0f7e4-184">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-184">Description</span></span> | <span data-ttu-id="0f7e4-185">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="0f7e4-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0f7e4-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="0f7e4-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="0f7e4-187">Richiede un indirizzo di posta elettronica confermato per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="0f7e4-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="0f7e4-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="0f7e4-189">Richiede un numero di telefono confermato per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="0f7e4-190">token</span><span class="sxs-lookup"><span data-stu-id="0f7e4-190">Tokens</span></span>

<span data-ttu-id="0f7e4-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) consente di specificare il [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="0f7e4-192">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="0f7e4-193">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="0f7e4-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0f7e4-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="0f7e4-195">Ottiene o imposta il `AuthenticatorTokenProvider` usato per convalidare gli accessi a due fattori con un autenticatore.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="0f7e4-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0f7e4-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="0f7e4-197">Ottiene o imposta il `ChangeEmailTokenProvider` usata per generare i token usati in messaggi di posta elettronica conferma Modifica messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="0f7e4-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0f7e4-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="0f7e4-199">Ottiene o imposta il `ChangePhoneNumberTokenProvider` usata per generare i token usati quando si modificano i numeri di telefono.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="0f7e4-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0f7e4-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="0f7e4-201">Ottiene o imposta il provider di token utilizzato per generare i token utilizzati nei messaggi di posta elettronica conferma account.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="0f7e4-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="0f7e4-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="0f7e4-203">Ottiene o imposta il [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usata per generare i token utilizzati nei messaggi di posta elettronica la reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="0f7e4-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="0f7e4-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="0f7e4-205">Consente di creare un [Provider di Token utente](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la chiave usata come nome del provider.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="0f7e4-206">Utente</span><span class="sxs-lookup"><span data-stu-id="0f7e4-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="0f7e4-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) consente di specificare il [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="0f7e4-208">Proprietà</span><span class="sxs-lookup"><span data-stu-id="0f7e4-208">Property</span></span> | <span data-ttu-id="0f7e4-209">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-209">Description</span></span> | <span data-ttu-id="0f7e4-210">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="0f7e4-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="0f7e4-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="0f7e4-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="0f7e4-212">Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-212">Allowed characters in the username.</span></span> | <span data-ttu-id="0f7e4-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="0f7e4-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="0f7e4-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="0f7e4-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="0f7e4-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="0f7e4-215">0123456789</span></span><br><span data-ttu-id="0f7e4-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="0f7e4-216">-.\_@+</span></span> |
| [<span data-ttu-id="0f7e4-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="0f7e4-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="0f7e4-218">Richiede che ogni utente un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="0f7e4-219">Impostazioni dei cookie</span><span class="sxs-lookup"><span data-stu-id="0f7e4-219">Cookie settings</span></span>

<span data-ttu-id="0f7e4-220">Configurare il cookie dell'app in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0f7e4-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) deve essere chiamato **dopo** chiamata `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="0f7e4-222">Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="0f7e4-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="0f7e4-223">Opzioni Hasher password</span><span class="sxs-lookup"><span data-stu-id="0f7e4-223">Password Hasher options</span></span>

<span data-ttu-id="0f7e4-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> Ottiene e imposta le opzioni per l'hashing della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="0f7e4-225">Opzione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-225">Option</span></span> | <span data-ttu-id="0f7e4-226">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f7e4-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="0f7e4-227">La modalità di compatibilità utilizzata durante l'hashing di nuove password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="0f7e4-228">Il valore predefinito è <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="0f7e4-229">Il primo byte di una password con hash, denominato un *marcatore formato*, specifica la versione dell'algoritmo di hash utilizzato per l'hash della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="0f7e4-230">Quando si verifica per determinare se una password rispetto a un hash, il <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> metodo seleziona dell'algoritmo corretto basato sul primo byte.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="0f7e4-231">Un client è in grado di autenticare indipendentemente dal fatto che di quale versione dell'algoritmo è stato usato per l'hash della password.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="0f7e4-232">L'impostazione della modalità di compatibilità influisce l'hashing delle *nuove password*.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="0f7e4-233">Il numero di iterazioni usate durante l'hashing delle password tramite PBKDF2.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="0f7e4-234">Questo valore viene utilizzato solo quando la <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> è impostata su <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="0f7e4-235">Il valore deve essere un numero intero positivo e il valore predefinito è `10000`.</span><span class="sxs-lookup"><span data-stu-id="0f7e4-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="0f7e4-236">Nell'esempio seguente, il <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> è impostata su `12000` in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0f7e4-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
