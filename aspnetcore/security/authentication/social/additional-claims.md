---
title: Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core
author: guardrex
description: Informazioni su come stabilire attestazioni aggiuntive e i token da provider esterni.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336189"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="37d63-103">Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37d63-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="37d63-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37d63-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="37d63-105">Un'app ASP.NET Core può stabilire altre attestazioni e token dai provider di autenticazione esterni, ad esempio Facebook, Google, Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="37d63-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="37d63-106">Ogni provider rivela varie informazioni relative agli utenti sulla piattaforma, ma il modello per la ricezione e trasformazione dei dati utente in attestazioni aggiuntive è la stessa.</span><span class="sxs-lookup"><span data-stu-id="37d63-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="37d63-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37d63-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="37d63-108">Prerequisito</span><span class="sxs-lookup"><span data-stu-id="37d63-108">Prerequisite</span></span>

<span data-ttu-id="37d63-109">Decidere quali provider di autenticazione esterni per il supporto nell'app.</span><span class="sxs-lookup"><span data-stu-id="37d63-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="37d63-110">Per ogni provider, registrare l'app e ottenere un ID client e segreto client.</span><span class="sxs-lookup"><span data-stu-id="37d63-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="37d63-111">Per altre informazioni, vedere <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="37d63-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="37d63-112">Il [app di esempio](#sample-app-instructions) Usa le [provider di autenticazione Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="37d63-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="37d63-113">Configurazione del provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="37d63-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="37d63-114">Impostare l'ID client e segreto client</span><span class="sxs-lookup"><span data-stu-id="37d63-114">Set the client ID and client secret</span></span>

<span data-ttu-id="37d63-115">Il provider di autenticazione OAuth stabilisce una relazione di trust con un'app usando un ID client e segreto client.</span><span class="sxs-lookup"><span data-stu-id="37d63-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="37d63-116">ID client e i valori del segreto client vengono creati per l'app dal provider di autenticazione esterna quando l'app viene registrata con il provider.</span><span class="sxs-lookup"><span data-stu-id="37d63-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="37d63-117">Ogni provider esterno che usa l'app deve essere configurato in modo indipendente con ID client e segreto client del provider.</span><span class="sxs-lookup"><span data-stu-id="37d63-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="37d63-118">Per altre informazioni, vedere gli argomenti di provider di autenticazione esterno che si applicano al proprio scenario:</span><span class="sxs-lookup"><span data-stu-id="37d63-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="37d63-119">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="37d63-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="37d63-120">Autenticazione Google</span><span class="sxs-lookup"><span data-stu-id="37d63-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="37d63-121">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="37d63-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="37d63-122">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="37d63-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="37d63-123">Altri provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="37d63-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="37d63-124">Openid Connect</span><span class="sxs-lookup"><span data-stu-id="37d63-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="37d63-125">L'app di esempio configura il provider di autenticazione Google con un ID client e segreto client fornito da Google:</span><span class="sxs-lookup"><span data-stu-id="37d63-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="37d63-126">Definire l'ambito di autenticazione</span><span class="sxs-lookup"><span data-stu-id="37d63-126">Establish the authentication scope</span></span>

<span data-ttu-id="37d63-127">Specificare l'elenco di autorizzazioni per recuperare dal provider, specificando il <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="37d63-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="37d63-128">Gli ambiti di autenticazione per comuni provider esterni vengono visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="37d63-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="37d63-129">Provider</span><span class="sxs-lookup"><span data-stu-id="37d63-129">Provider</span></span>  | <span data-ttu-id="37d63-130">Ambito</span><span class="sxs-lookup"><span data-stu-id="37d63-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="37d63-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="37d63-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="37d63-132">Google</span><span class="sxs-lookup"><span data-stu-id="37d63-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="37d63-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="37d63-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="37d63-134">Twitter</span><span class="sxs-lookup"><span data-stu-id="37d63-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="37d63-135">L'app di esempio aggiunge Google `plus.login` ambito alla richiesta di accesso Google + autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="37d63-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="37d63-136">Eseguire il mapping di chiavi di dati utente e crea le attestazioni</span><span class="sxs-lookup"><span data-stu-id="37d63-136">Map user data keys and create claims</span></span>

<span data-ttu-id="37d63-137">Nelle opzioni del provider, specificare un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> per ogni chiave nei dati di utente del provider esterno JSON per l'identità dell'app per la lettura su Accedi.</span><span class="sxs-lookup"><span data-stu-id="37d63-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="37d63-138">Per altre informazioni sui tipi di attestazione, vedere <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="37d63-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="37d63-139">L'app di esempio crea un <xref:System.Security.Claims.ClaimTypes.Gender> attestazione dal `gender` chiave nei dati utente Google:</span><span class="sxs-lookup"><span data-stu-id="37d63-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="37d63-140">Nelle <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un' <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) viene eseguito l'accesso all'App con <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="37d63-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="37d63-141">Durante il processo di accesso di <xref:Microsoft.AspNetCore.Identity.UserManager`1> può archiviare un `ApplicationUser` attestazioni per i dati utente disponibili dal <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="37d63-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="37d63-142">Nell'app di esempio, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) stabilisce una <xref:System.Security.Claims.ClaimTypes.Gender> attestazioni per i signed in `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="37d63-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="37d63-143">Salvare il token di accesso</span><span class="sxs-lookup"><span data-stu-id="37d63-143">Save the access token</span></span>

<span data-ttu-id="37d63-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Definisce se i token di accesso e di aggiornamento devono essere archiviati nel <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> un'autorizzazione ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="37d63-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="37d63-145">`SaveTokens` è impostato su `false` per impostazione predefinita per ridurre le dimensioni del cookie di autenticazione finale.</span><span class="sxs-lookup"><span data-stu-id="37d63-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="37d63-146">L'app di esempio imposta il valore della `SaveTokens` al `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="37d63-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="37d63-147">Quando `OnPostConfirmationAsync` viene eseguito, archiviare il token di accesso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dal provider esterno nel `ApplicationUser`del `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="37d63-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="37d63-148">L'app di esempio consente di salvare il token di accesso in:</span><span class="sxs-lookup"><span data-stu-id="37d63-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="37d63-149">`OnPostConfirmationAsync` &ndash; Viene eseguito per la registrazione di nuovi utenti.</span><span class="sxs-lookup"><span data-stu-id="37d63-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="37d63-150">`OnGetCallbackAsync` &ndash; Viene eseguito quando un utente registrato in precedenza accede all'app.</span><span class="sxs-lookup"><span data-stu-id="37d63-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="37d63-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="37d63-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="37d63-152">Come aggiungere token personalizzati aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="37d63-152">How to add additional custom tokens</span></span>

<span data-ttu-id="37d63-153">Per illustrare come aggiungere un token personalizzato, che viene archiviato come parte del `SaveTokens`, l'app di esempio aggiunge un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con l'attuale <xref:System.DateTime> per un' [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) di `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="37d63-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="37d63-154">Istruzioni di app di esempio</span><span class="sxs-lookup"><span data-stu-id="37d63-154">Sample app instructions</span></span>

<span data-ttu-id="37d63-155">L'app di esempio viene illustrato come:</span><span class="sxs-lookup"><span data-stu-id="37d63-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="37d63-156">Ottenere il sesso dell'utente da Google e archiviare un'attestazione di sesso con il valore.</span><span class="sxs-lookup"><span data-stu-id="37d63-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="37d63-157">Store il token di accesso di Google dell'utente `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="37d63-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="37d63-158">Per usare l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="37d63-158">To use the sample app:</span></span>

1. <span data-ttu-id="37d63-159">Registrare l'app e ottenere un ID client valido e il segreto client per l'autenticazione Google.</span><span class="sxs-lookup"><span data-stu-id="37d63-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="37d63-160">Per altre informazioni, vedere <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="37d63-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="37d63-161">Fornire l'ID client e segreto client per l'app nel <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> di `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37d63-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="37d63-162">Eseguire l'app e richiedere la pagina My attestazioni.</span><span class="sxs-lookup"><span data-stu-id="37d63-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="37d63-163">Quando l'utente non viene eseguito l'accesso, l'app reindirizza a Google.</span><span class="sxs-lookup"><span data-stu-id="37d63-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="37d63-164">Accedi con Google.</span><span class="sxs-lookup"><span data-stu-id="37d63-164">Sign in with Google.</span></span> <span data-ttu-id="37d63-165">Google reindirizza l'utente all'App (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="37d63-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="37d63-166">L'utente viene autenticato e viene caricata la pagina My attestazioni.</span><span class="sxs-lookup"><span data-stu-id="37d63-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="37d63-167">L'attestazione relativa al sesso è presente in **attestazioni utente** con il valore ottenuto da Google.</span><span class="sxs-lookup"><span data-stu-id="37d63-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="37d63-168">Il token di accesso viene visualizzato nei **le proprietà di autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="37d63-168">The access token appears in the **Authentication Properties**.</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
