---
title: Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core
author: guardrex
description: Informazioni su come stabilire attestazioni aggiuntive e i token da provider esterni.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874850"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="a33ea-103">Rendere persistenti le attestazioni aggiuntive e i token da provider esterni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a33ea-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="a33ea-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a33ea-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a33ea-105">Un'app ASP.NET Core può stabilire altre attestazioni e token dai provider di autenticazione esterni, ad esempio Facebook, Google, Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="a33ea-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="a33ea-106">Ogni provider rivela varie informazioni relative agli utenti sulla piattaforma, ma il modello per la ricezione e trasformazione dei dati utente in attestazioni aggiuntive è la stessa.</span><span class="sxs-lookup"><span data-stu-id="a33ea-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="a33ea-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a33ea-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a33ea-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a33ea-108">Prerequisites</span></span>

<span data-ttu-id="a33ea-109">Decidere quali provider di autenticazione esterni per il supporto nell'app.</span><span class="sxs-lookup"><span data-stu-id="a33ea-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="a33ea-110">Per ogni provider, registrare l'app e ottenere un ID client e segreto client.</span><span class="sxs-lookup"><span data-stu-id="a33ea-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="a33ea-111">Per altre informazioni, vedere <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="a33ea-112">L'app di esempio Usa la [provider di autenticazione Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="a33ea-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="a33ea-113">Impostare l'ID client e segreto client</span><span class="sxs-lookup"><span data-stu-id="a33ea-113">Set the client ID and client secret</span></span>

<span data-ttu-id="a33ea-114">Il provider di autenticazione OAuth stabilisce una relazione di trust con un'app usando un ID client e segreto client.</span><span class="sxs-lookup"><span data-stu-id="a33ea-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="a33ea-115">ID client e i valori del segreto client vengono creati per l'app dal provider di autenticazione esterna quando l'app viene registrata con il provider.</span><span class="sxs-lookup"><span data-stu-id="a33ea-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="a33ea-116">Ogni provider esterno che usa l'app deve essere configurato in modo indipendente con ID client e segreto client del provider.</span><span class="sxs-lookup"><span data-stu-id="a33ea-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="a33ea-117">Per altre informazioni, vedere gli argomenti di provider di autenticazione esterno che si applicano al proprio scenario:</span><span class="sxs-lookup"><span data-stu-id="a33ea-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="a33ea-118">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="a33ea-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="a33ea-119">Autenticazione Google</span><span class="sxs-lookup"><span data-stu-id="a33ea-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="a33ea-120">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="a33ea-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="a33ea-121">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="a33ea-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="a33ea-122">Altri provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="a33ea-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="a33ea-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="a33ea-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="a33ea-124">L'app di esempio configura il provider di autenticazione Google con un ID client e segreto client fornito da Google:</span><span class="sxs-lookup"><span data-stu-id="a33ea-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="a33ea-125">Definire l'ambito di autenticazione</span><span class="sxs-lookup"><span data-stu-id="a33ea-125">Establish the authentication scope</span></span>

<span data-ttu-id="a33ea-126">Specificare l'elenco di autorizzazioni per recuperare dal provider, specificando il <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="a33ea-127">Gli ambiti di autenticazione per comuni provider esterni vengono visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="a33ea-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="a33ea-128">Provider</span><span class="sxs-lookup"><span data-stu-id="a33ea-128">Provider</span></span>  | <span data-ttu-id="a33ea-129">Ambito</span><span class="sxs-lookup"><span data-stu-id="a33ea-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="a33ea-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="a33ea-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="a33ea-131">Google</span><span class="sxs-lookup"><span data-stu-id="a33ea-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="a33ea-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="a33ea-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="a33ea-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="a33ea-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="a33ea-134">Nell'app di esempio, Google `userinfo.profile` ambito viene aggiunto automaticamente dal framework quando <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> viene chiamato sul <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="a33ea-135">Se l'app richiede altri ambiti, aggiungerli alle opzioni.</span><span class="sxs-lookup"><span data-stu-id="a33ea-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="a33ea-136">Nell'esempio seguente, Google `https://www.googleapis.com/auth/user.birthday.read` ambito viene aggiunto per poter recuperare data di nascita dell'utente:</span><span class="sxs-lookup"><span data-stu-id="a33ea-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="a33ea-137">Eseguire il mapping di chiavi di dati utente e crea le attestazioni</span><span class="sxs-lookup"><span data-stu-id="a33ea-137">Map user data keys and create claims</span></span>

<span data-ttu-id="a33ea-138">Nelle opzioni del provider, specificare una <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> per ogni chiave/sottochiave nei dati di utente del provider esterno JSON per l'identità dell'app per la lettura su Accedi.</span><span class="sxs-lookup"><span data-stu-id="a33ea-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="a33ea-139">Per altre informazioni sui tipi di attestazione, vedere <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="a33ea-140">L'app di esempio crea le impostazioni locali (`urn:google:locale`) e l'immagine (`urn:google:picture`) di attestazioni dal `locale` e `picture` chiavi nei dati utente Google:</span><span class="sxs-lookup"><span data-stu-id="a33ea-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="a33ea-141">Nelle <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un' <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) viene eseguito l'accesso all'App con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="a33ea-142">Durante il processo di accesso di <xref:Microsoft.AspNetCore.Identity.UserManager%601> può archiviare un `ApplicationUser` attestazioni per i dati utente disponibile dal <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="a33ea-143">Nell'app di esempio, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) stabilisce le impostazioni locali (`urn:google:locale`) e l'immagine (`urn:google:picture`) le attestazioni per i signed in `ApplicationUser`, tra cui un'attestazione per <xref:System.Security.Claims.ClaimTypes.GivenName> :</span><span class="sxs-lookup"><span data-stu-id="a33ea-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="a33ea-144">Per impostazione predefinita, le attestazioni dell'utente vengono archiviate nel cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a33ea-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="a33ea-145">Se il cookie di autenticazione è troppo grande, è possibile che l'app a non riuscire perché:</span><span class="sxs-lookup"><span data-stu-id="a33ea-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="a33ea-146">Il browser rileva che l'intestazione del cookie è troppo lungo.</span><span class="sxs-lookup"><span data-stu-id="a33ea-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="a33ea-147">La dimensione complessiva della richiesta è troppo grande.</span><span class="sxs-lookup"><span data-stu-id="a33ea-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="a33ea-148">Se una grande quantità di dati dell'utente è obbligatorio per l'elaborazione delle richieste utente:</span><span class="sxs-lookup"><span data-stu-id="a33ea-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="a33ea-149">Limitare il numero e dimensione delle attestazioni utente per a ciò che l'app richiede solo di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="a33ea-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="a33ea-150">Usare una classe personalizzata <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> per il Middleware di autenticazione Cookie <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> per archiviare l'identità tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="a33ea-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="a33ea-151">Mantenere grandi quantità di informazioni sull'identità nel server durante l'invio solo di una chiave di identificatore di sessione di piccole dimensioni al client.</span><span class="sxs-lookup"><span data-stu-id="a33ea-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="a33ea-152">Salvare il token di accesso</span><span class="sxs-lookup"><span data-stu-id="a33ea-152">Save the access token</span></span>

<span data-ttu-id="a33ea-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> Definisce se i token di accesso e di aggiornamento devono essere archiviati nel <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> un'autorizzazione ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="a33ea-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="a33ea-154">`SaveTokens` è impostato su `false` per impostazione predefinita per ridurre le dimensioni del cookie di autenticazione finale.</span><span class="sxs-lookup"><span data-stu-id="a33ea-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="a33ea-155">L'app di esempio imposta il valore della `SaveTokens` al `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="a33ea-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="a33ea-156">Quando `OnPostConfirmationAsync` viene eseguito, archiviare il token di accesso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dal provider esterno nel `ApplicationUser`del `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="a33ea-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="a33ea-157">L'app di esempio consente di salvare il token di accesso nel `OnPostConfirmationAsync` (registrazione di nuovi utenti) e `OnGetCallbackAsync` (utente registrato in precedenza) nella *Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a33ea-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="a33ea-158">Come aggiungere token personalizzati aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="a33ea-158">How to add additional custom tokens</span></span>

<span data-ttu-id="a33ea-159">Per illustrare come aggiungere un token personalizzato, che viene archiviato come parte del `SaveTokens`, l'app di esempio aggiunge un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con l'attuale <xref:System.DateTime> per un' [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) di `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="a33ea-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="a33ea-160">Creazione e aggiunta di attestazioni</span><span class="sxs-lookup"><span data-stu-id="a33ea-160">Creating and adding claims</span></span>

<span data-ttu-id="a33ea-161">Il framework fornisce azioni comuni e i metodi di estensione per la creazione e aggiunta di attestazioni all'insieme.</span><span class="sxs-lookup"><span data-stu-id="a33ea-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="a33ea-162">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> e <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="a33ea-163">Gli utenti possono definire le azioni personalizzate derivandole dalla <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e l'implementazione astratta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> (metodo).</span><span class="sxs-lookup"><span data-stu-id="a33ea-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="a33ea-164">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="a33ea-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="a33ea-165">Rimozione di azioni di attestazione e attestazioni</span><span class="sxs-lookup"><span data-stu-id="a33ea-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="a33ea-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) rimuove tutte le attestazioni azioni per il dato <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> dalla raccolta.</span><span class="sxs-lookup"><span data-stu-id="a33ea-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="a33ea-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) consente di eliminare un'attestazione di dato <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> dall'identità.</span><span class="sxs-lookup"><span data-stu-id="a33ea-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="a33ea-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> viene usato principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) rimuovere attestazioni generati dal protocollo.</span><span class="sxs-lookup"><span data-stu-id="a33ea-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="a33ea-169">Output di app di esempio</span><span class="sxs-lookup"><span data-stu-id="a33ea-169">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a><span data-ttu-id="a33ea-170">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a33ea-170">Additional resources</span></span>

* <span data-ttu-id="a33ea-171">[app SocialSample engineering ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l'app di esempio collegata è nel [del repository di GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` ramo engineering.</span><span class="sxs-lookup"><span data-stu-id="a33ea-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="a33ea-172">Il `master` ramo contiene il codice in fase di sviluppo della prossima versione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a33ea-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="a33ea-173">Per visualizzare una versione dell'app di esempio per una versione rilasciata di ASP.NET Core, usare il **ramo** menu a discesa per selezionare un ramo di rilascio (ad esempio `release/2.2`).</span><span class="sxs-lookup"><span data-stu-id="a33ea-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
