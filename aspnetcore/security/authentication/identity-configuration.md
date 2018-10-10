---
title: Configurare ASP.NET Core Identity
author: AdrienTorris
description: Informazioni sui valori predefiniti di ASP.NET Core Identity e su come configurare le proprietà di identità per usare valori personalizzati.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 02441cd28c2a99eda7b50ed54f4437d4b52ca5d9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911943"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="6a341-103">Configurare ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="6a341-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="6a341-104">ASP.NET Core Identity Usa valori predefiniti per le impostazioni, ad esempio criteri password, blocco e la configurazione di cookie.</span><span class="sxs-lookup"><span data-stu-id="6a341-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="6a341-105">Queste impostazioni possono essere sostituite nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="6a341-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="6a341-106">Opzioni di identità</span><span class="sxs-lookup"><span data-stu-id="6a341-106">Identity options</span></span>

<span data-ttu-id="6a341-107">Il [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe rappresenta le opzioni che possono essere usate per configurare il sistema di identità.</span><span class="sxs-lookup"><span data-stu-id="6a341-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="6a341-108">`IdentityOptions` deve essere impostata **dopo aver** chiamata `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="6a341-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="6a341-109">Identità delle attestazioni</span><span class="sxs-lookup"><span data-stu-id="6a341-109">Claims Identity</span></span>

<span data-ttu-id="6a341-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) consente di specificare il [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con le proprietà visualizzate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="6a341-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="6a341-111">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-111">Property</span></span> | <span data-ttu-id="6a341-112">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-112">Description</span></span> | <span data-ttu-id="6a341-113">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6a341-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="6a341-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="6a341-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="6a341-115">Ottiene o imposta il tipo di attestazione utilizzato per un'attestazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="6a341-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="6a341-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="6a341-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="6a341-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="6a341-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="6a341-118">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione di timestamp di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="6a341-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="6a341-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="6a341-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="6a341-120">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione dell'identificatore utente.</span><span class="sxs-lookup"><span data-stu-id="6a341-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="6a341-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="6a341-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="6a341-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="6a341-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="6a341-123">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione del nome utente.</span><span class="sxs-lookup"><span data-stu-id="6a341-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="6a341-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="6a341-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="6a341-125">Blocco</span><span class="sxs-lookup"><span data-stu-id="6a341-125">Lockout</span></span>

<span data-ttu-id="6a341-126">Blocco viene impostato nel [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metodo:</span><span class="sxs-lookup"><span data-stu-id="6a341-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="6a341-127">Il codice precedente si basa sul `Login` modello di identità.</span><span class="sxs-lookup"><span data-stu-id="6a341-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="6a341-128">Le opzioni di blocco sono impostate `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6a341-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="6a341-129">Il codice precedente imposta la [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="6a341-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="6a341-130">Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio.</span><span class="sxs-lookup"><span data-stu-id="6a341-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="6a341-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) consente di specificare il [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="6a341-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="6a341-132">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-132">Property</span></span> | <span data-ttu-id="6a341-133">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-133">Description</span></span> | <span data-ttu-id="6a341-134">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6a341-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="6a341-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="6a341-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="6a341-136">Determina se un nuovo utente può essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="6a341-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="6a341-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="6a341-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="6a341-138">Quanto tempo un utente viene bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="6a341-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="6a341-139">5 minuti</span><span class="sxs-lookup"><span data-stu-id="6a341-139">5 minutes</span></span> |
| [<span data-ttu-id="6a341-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="6a341-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="6a341-141">Il numero di tentativi di accesso non riuscito fino a quando un utente è bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="6a341-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="6a341-142">5</span><span class="sxs-lookup"><span data-stu-id="6a341-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="6a341-143">Password</span><span class="sxs-lookup"><span data-stu-id="6a341-143">Password</span></span>

<span data-ttu-id="6a341-144">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, lettera minuscola, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="6a341-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="6a341-145">Le password devono essere composta da almeno sei caratteri.</span><span class="sxs-lookup"><span data-stu-id="6a341-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="6a341-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) possono essere impostate nella `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6a341-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="6a341-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) consente di specificare il [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="6a341-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="6a341-148">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-148">Property</span></span> | <span data-ttu-id="6a341-149">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-149">Description</span></span> | <span data-ttu-id="6a341-150">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6a341-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="6a341-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="6a341-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="6a341-152">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="6a341-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="6a341-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="6a341-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="6a341-154">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-154">The minimum length of the password.</span></span> | <span data-ttu-id="6a341-155">6</span><span class="sxs-lookup"><span data-stu-id="6a341-155">6</span></span> |
| [<span data-ttu-id="6a341-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="6a341-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="6a341-157">Richiede un carattere minuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="6a341-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="6a341-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="6a341-159">Richiede un carattere non alfanumerico della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="6a341-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="6a341-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="6a341-161">Si applica solo a ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6a341-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="6a341-162">Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="6a341-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="6a341-163">1</span><span class="sxs-lookup"><span data-stu-id="6a341-163">1</span></span> |
| [<span data-ttu-id="6a341-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="6a341-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="6a341-165">Richiede un carattere maiuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="6a341-166">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-166">Property</span></span> | <span data-ttu-id="6a341-167">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-167">Description</span></span> | <span data-ttu-id="6a341-168">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6a341-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="6a341-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="6a341-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="6a341-170">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="6a341-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="6a341-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="6a341-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="6a341-172">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-172">The minimum length of the password.</span></span> | <span data-ttu-id="6a341-173">6</span><span class="sxs-lookup"><span data-stu-id="6a341-173">6</span></span> |
| [<span data-ttu-id="6a341-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="6a341-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="6a341-175">Richiede un carattere minuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="6a341-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="6a341-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="6a341-177">Richiede un carattere non alfanumerico della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="6a341-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="6a341-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="6a341-179">Richiede un carattere maiuscolo della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="6a341-180">Accedi</span><span class="sxs-lookup"><span data-stu-id="6a341-180">Sign-in</span></span>

<span data-ttu-id="6a341-181">Il codice seguente imposta `SignIn` impostazioni (impostazione predefinita valori):</span><span class="sxs-lookup"><span data-stu-id="6a341-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="6a341-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) consente di specificare il [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="6a341-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="6a341-183">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-183">Property</span></span> | <span data-ttu-id="6a341-184">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-184">Description</span></span> | <span data-ttu-id="6a341-185">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6a341-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="6a341-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="6a341-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="6a341-187">Richiede un indirizzo di posta elettronica confermato per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="6a341-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="6a341-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="6a341-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="6a341-189">Richiede un numero di telefono confermato per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="6a341-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="6a341-190">token</span><span class="sxs-lookup"><span data-stu-id="6a341-190">Tokens</span></span>

<span data-ttu-id="6a341-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) consente di specificare il [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="6a341-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="6a341-192">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="6a341-193">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="6a341-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="6a341-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="6a341-195">Ottiene o imposta il `AuthenticatorTokenProvider` usato per convalidare gli accessi a due fattori con un autenticatore.</span><span class="sxs-lookup"><span data-stu-id="6a341-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="6a341-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="6a341-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="6a341-197">Ottiene o imposta il `ChangeEmailTokenProvider` usata per generare i token usati in messaggi di posta elettronica conferma Modifica messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="6a341-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="6a341-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="6a341-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="6a341-199">Ottiene o imposta il `ChangePhoneNumberTokenProvider` usata per generare i token usati quando si modificano i numeri di telefono.</span><span class="sxs-lookup"><span data-stu-id="6a341-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="6a341-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="6a341-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="6a341-201">Ottiene o imposta il provider di token utilizzato per generare i token utilizzati nei messaggi di posta elettronica conferma account.</span><span class="sxs-lookup"><span data-stu-id="6a341-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="6a341-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="6a341-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="6a341-203">Ottiene o imposta il [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usata per generare i token utilizzati nei messaggi di posta elettronica la reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="6a341-203">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="6a341-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="6a341-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="6a341-205">Consente di creare un [Provider di Token utente](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la chiave usata come nome del provider.</span><span class="sxs-lookup"><span data-stu-id="6a341-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="6a341-206">Utente</span><span class="sxs-lookup"><span data-stu-id="6a341-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="6a341-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) consente di specificare il [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="6a341-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="6a341-208">Proprietà</span><span class="sxs-lookup"><span data-stu-id="6a341-208">Property</span></span> | <span data-ttu-id="6a341-209">Description</span><span class="sxs-lookup"><span data-stu-id="6a341-209">Description</span></span> | <span data-ttu-id="6a341-210">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6a341-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="6a341-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="6a341-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="6a341-212">Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="6a341-212">Allowed characters in the username.</span></span> | <span data-ttu-id="6a341-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="6a341-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="6a341-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="6a341-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="6a341-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="6a341-215">0123456789</span></span><br><span data-ttu-id="6a341-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="6a341-216">-.\_@+</span></span> |
| [<span data-ttu-id="6a341-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="6a341-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="6a341-218">Richiede che ogni utente un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="6a341-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="6a341-219">Impostazioni dei cookie</span><span class="sxs-lookup"><span data-stu-id="6a341-219">Cookie settings</span></span>

<span data-ttu-id="6a341-220">Configurare il cookie dell'app in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6a341-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6a341-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) deve essere chiamato **dopo** chiamata `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="6a341-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="6a341-222">Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="6a341-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>
