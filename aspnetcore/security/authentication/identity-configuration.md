---
title: Configurare ASP.NET Core Identity
author: AdrienTorris
description: Informazioni sui valori predefiniti di ASP.NET Core Identity e su come configurare le proprietà di identità per usare valori personalizzati.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: c597eacbb21ed0968e6195f7b6dcb46d37ba80a5
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870917"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="bb59f-103">Configurare ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="bb59f-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="bb59f-104">ASP.NET Core Identity Usa valori predefiniti per le impostazioni, ad esempio criteri password, blocco e la configurazione di cookie.</span><span class="sxs-lookup"><span data-stu-id="bb59f-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="bb59f-105">Queste impostazioni possono essere sostituite nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="bb59f-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="bb59f-106">Opzioni di identità</span><span class="sxs-lookup"><span data-stu-id="bb59f-106">Identity options</span></span>

<span data-ttu-id="bb59f-107">Il [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe rappresenta le opzioni che possono essere usate per configurare il sistema di identità.</span><span class="sxs-lookup"><span data-stu-id="bb59f-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="bb59f-108">`IdentityOptions` deve essere impostata **dopo aver** chiamata `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="bb59f-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="bb59f-109">Identità delle attestazioni</span><span class="sxs-lookup"><span data-stu-id="bb59f-109">Claims Identity</span></span>

<span data-ttu-id="bb59f-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) consente di specificare il [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con le proprietà visualizzate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="bb59f-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="bb59f-111">Proprietà</span><span class="sxs-lookup"><span data-stu-id="bb59f-111">Property</span></span> | <span data-ttu-id="bb59f-112">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bb59f-112">Description</span></span> | <span data-ttu-id="bb59f-113">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="bb59f-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bb59f-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="bb59f-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="bb59f-115">Ottiene o imposta il tipo di attestazione utilizzato per un'attestazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="bb59f-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="bb59f-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="bb59f-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="bb59f-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="bb59f-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="bb59f-118">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione di timestamp di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="bb59f-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="bb59f-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="bb59f-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="bb59f-120">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione dell'identificatore utente.</span><span class="sxs-lookup"><span data-stu-id="bb59f-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="bb59f-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="bb59f-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="bb59f-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="bb59f-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="bb59f-123">Ottiene o imposta il tipo di attestazione utilizzato per l'attestazione del nome utente.</span><span class="sxs-lookup"><span data-stu-id="bb59f-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="bb59f-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="bb59f-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="bb59f-125">Blocco</span><span class="sxs-lookup"><span data-stu-id="bb59f-125">Lockout</span></span>

<span data-ttu-id="bb59f-126">Blocco viene impostato nel [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metodo:</span><span class="sxs-lookup"><span data-stu-id="bb59f-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="bb59f-127">Il codice precedente si basa sul `Login` modello di identità.</span><span class="sxs-lookup"><span data-stu-id="bb59f-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="bb59f-128">Le opzioni di blocco sono impostate `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bb59f-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="bb59f-129">Il codice precedente imposta la [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="bb59f-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="bb59f-130">Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio.</span><span class="sxs-lookup"><span data-stu-id="bb59f-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="bb59f-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) consente di specificare il [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bb59f-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bb59f-132">Proprietà</span><span class="sxs-lookup"><span data-stu-id="bb59f-132">Property</span></span> | <span data-ttu-id="bb59f-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bb59f-133">Description</span></span> | <span data-ttu-id="bb59f-134">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="bb59f-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bb59f-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="bb59f-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="bb59f-136">Determina se un nuovo utente può essere bloccato.</span><span class="sxs-lookup"><span data-stu-id="bb59f-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="bb59f-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="bb59f-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="bb59f-138">Quanto tempo un utente viene bloccato quando si verifica un blocco.</span><span class="sxs-lookup"><span data-stu-id="bb59f-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="bb59f-139">5 minuti</span><span class="sxs-lookup"><span data-stu-id="bb59f-139">5 minutes</span></span> |
| [<span data-ttu-id="bb59f-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="bb59f-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="bb59f-141">Il numero di tentativi di accesso non riuscito fino a quando un utente è bloccato, se il blocco è abilitato.</span><span class="sxs-lookup"><span data-stu-id="bb59f-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="bb59f-142">5</span><span class="sxs-lookup"><span data-stu-id="bb59f-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="bb59f-143">Password</span><span class="sxs-lookup"><span data-stu-id="bb59f-143">Password</span></span>

<span data-ttu-id="bb59f-144">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, lettera minuscola, una cifra e un carattere non alfanumerico.</span><span class="sxs-lookup"><span data-stu-id="bb59f-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="bb59f-145">Le password devono essere composta da almeno sei caratteri.</span><span class="sxs-lookup"><span data-stu-id="bb59f-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="bb59f-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) possono essere impostate nella `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bb59f-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="bb59f-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) consente di specificare il [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bb59f-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bb59f-148">Proprietà</span><span class="sxs-lookup"><span data-stu-id="bb59f-148">Property</span></span> | <span data-ttu-id="bb59f-149">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bb59f-149">Description</span></span> | <span data-ttu-id="bb59f-150">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="bb59f-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bb59f-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="bb59f-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="bb59f-152">Richiede un numero compreso tra 0 e 9 nella password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="bb59f-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="bb59f-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="bb59f-154">La lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-154">The minimum length of the password.</span></span> | <span data-ttu-id="bb59f-155">6</span><span class="sxs-lookup"><span data-stu-id="bb59f-155">6</span></span> |

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bb59f-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Si applica solo ad ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="bb59f-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="bb59f-157">Richiede il numero di caratteri distinti nella password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-157">Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="bb59f-158">| 1 |</span><span class="sxs-lookup"><span data-stu-id="bb59f-158">| 1 |</span></span>

::: moniker-end

<span data-ttu-id="bb59f-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Richiede una lettera minuscola nella password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requires a lowercase character in the password.</span></span><span data-ttu-id="bb59f-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Richiede un carattere non alfanumerici nella password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requires a non-alphanumeric character in the password.</span></span><span data-ttu-id="bb59f-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Richiede una lettera maiuscola nella password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requires an uppercase character in the password.</span></span> | `true` |

### <a name="sign-in"></a><span data-ttu-id="bb59f-162">Accedi</span><span class="sxs-lookup"><span data-stu-id="bb59f-162">Sign-in</span></span>

<span data-ttu-id="bb59f-163">Il codice seguente imposta `SignIn` impostazioni (impostazione predefinita valori):</span><span class="sxs-lookup"><span data-stu-id="bb59f-163">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="bb59f-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) consente di specificare il [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bb59f-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bb59f-165">Proprietà</span><span class="sxs-lookup"><span data-stu-id="bb59f-165">Property</span></span> | <span data-ttu-id="bb59f-166">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bb59f-166">Description</span></span> | <span data-ttu-id="bb59f-167">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="bb59f-167">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bb59f-168">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="bb59f-168">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="bb59f-169">Richiede un indirizzo di posta elettronica confermato per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="bb59f-169">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="bb59f-170">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="bb59f-170">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="bb59f-171">Richiede un numero di telefono confermato per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="bb59f-171">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="bb59f-172">token</span><span class="sxs-lookup"><span data-stu-id="bb59f-172">Tokens</span></span>

<span data-ttu-id="bb59f-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) consente di specificare il [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bb59f-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="bb59f-174">Proprietà</span><span class="sxs-lookup"><span data-stu-id="bb59f-174">Property</span></span>                                                         |                                                                                      <span data-ttu-id="bb59f-175">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bb59f-175">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="bb59f-176">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bb59f-176">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="bb59f-177">Ottiene o imposta il `AuthenticatorTokenProvider` usato per convalidare gli accessi a due fattori con un autenticatore.</span><span class="sxs-lookup"><span data-stu-id="bb59f-177">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="bb59f-178">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bb59f-178">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="bb59f-179">Ottiene o imposta il `ChangeEmailTokenProvider` usata per generare i token usati in messaggi di posta elettronica conferma Modifica messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="bb59f-179">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="bb59f-180">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bb59f-180">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="bb59f-181">Ottiene o imposta il `ChangePhoneNumberTokenProvider` usata per generare i token usati quando si modificano i numeri di telefono.</span><span class="sxs-lookup"><span data-stu-id="bb59f-181">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="bb59f-182">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bb59f-182">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="bb59f-183">Ottiene o imposta il provider di token utilizzato per generare i token utilizzati nei messaggi di posta elettronica conferma account.</span><span class="sxs-lookup"><span data-stu-id="bb59f-183">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="bb59f-184">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bb59f-184">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="bb59f-185">Ottiene o imposta il [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usata per generare i token utilizzati nei messaggi di posta elettronica la reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="bb59f-185">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="bb59f-186">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="bb59f-186">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="bb59f-187">Consente di creare un [Provider di Token utente](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la chiave usata come nome del provider.</span><span class="sxs-lookup"><span data-stu-id="bb59f-187">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="bb59f-188">Utente</span><span class="sxs-lookup"><span data-stu-id="bb59f-188">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="bb59f-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) consente di specificare il [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con le proprietà visualizzate nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bb59f-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bb59f-190">Proprietà</span><span class="sxs-lookup"><span data-stu-id="bb59f-190">Property</span></span> | <span data-ttu-id="bb59f-191">Descrizione</span><span class="sxs-lookup"><span data-stu-id="bb59f-191">Description</span></span> | <span data-ttu-id="bb59f-192">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="bb59f-192">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bb59f-193">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="bb59f-193">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="bb59f-194">Caratteri consentiti nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="bb59f-194">Allowed characters in the username.</span></span> | <span data-ttu-id="bb59f-195">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="bb59f-195">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="bb59f-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="bb59f-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="bb59f-197">0123456789</span><span class="sxs-lookup"><span data-stu-id="bb59f-197">0123456789</span></span><br><span data-ttu-id="bb59f-198">-._@+</span><span class="sxs-lookup"><span data-stu-id="bb59f-198">-._@+</span></span> |
| [<span data-ttu-id="bb59f-199">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="bb59f-199">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="bb59f-200">Richiede che ogni utente un messaggio di posta elettronica univoco.</span><span class="sxs-lookup"><span data-stu-id="bb59f-200">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="bb59f-201">Impostazioni dei cookie</span><span class="sxs-lookup"><span data-stu-id="bb59f-201">Cookie settings</span></span>

<span data-ttu-id="bb59f-202">Configurare il cookie dell'app in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bb59f-202">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bb59f-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) deve essere chiamato **dopo** chiamata `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="bb59f-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="bb59f-204">Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="bb59f-204">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>