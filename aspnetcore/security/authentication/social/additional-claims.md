---
title: Mantieni attestazioni e token aggiuntivi da provider esterni in ASP.NET Core
author: guardrex
description: Informazioni su come creare attestazioni e token aggiuntivi da provider esterni.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 44b3e72085e6265319b53b548f7f7ddde2adbd14
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828581"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="b1956-103">Mantieni attestazioni e token aggiuntivi da provider esterni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1956-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="b1956-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b1956-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1956-105">Un'app ASP.NET Core può creare attestazioni e token aggiuntivi da provider di autenticazione esterni, ad esempio Facebook, Google, Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="b1956-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="b1956-106">Ogni provider rivela informazioni diverse sugli utenti sulla propria piattaforma, ma il modello per la ricezione e la trasformazione dei dati utente in attestazioni aggiuntive è lo stesso.</span><span class="sxs-lookup"><span data-stu-id="b1956-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="b1956-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b1956-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1956-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b1956-108">Prerequisites</span></span>

<span data-ttu-id="b1956-109">Decidere quali provider di autenticazione esterni supportare nell'app.</span><span class="sxs-lookup"><span data-stu-id="b1956-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="b1956-110">Per ogni provider, registrare l'app e ottenere un ID client e un segreto client.</span><span class="sxs-lookup"><span data-stu-id="b1956-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="b1956-111">Per ulteriori informazioni, vedere <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="b1956-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="b1956-112">L'app di esempio usa il [provider di autenticazione Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="b1956-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="b1956-113">Impostare l'ID client e il segreto client</span><span class="sxs-lookup"><span data-stu-id="b1956-113">Set the client ID and client secret</span></span>

<span data-ttu-id="b1956-114">Il provider di autenticazione OAuth stabilisce una relazione di trust con un'app che usa un ID client e un segreto client.</span><span class="sxs-lookup"><span data-stu-id="b1956-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="b1956-115">I valori relativi all'ID client e al segreto client vengono creati per l'app dal provider di autenticazione esterno quando l'app viene registrata con il provider.</span><span class="sxs-lookup"><span data-stu-id="b1956-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="b1956-116">Ogni provider esterno usato dall'app deve essere configurato in modo indipendente con l'ID client e il segreto client del provider.</span><span class="sxs-lookup"><span data-stu-id="b1956-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="b1956-117">Per ulteriori informazioni, vedere gli argomenti del provider di autenticazione esterno applicabili allo scenario:</span><span class="sxs-lookup"><span data-stu-id="b1956-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="b1956-118">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="b1956-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="b1956-119">Autenticazione Google</span><span class="sxs-lookup"><span data-stu-id="b1956-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="b1956-120">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="b1956-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="b1956-121">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="b1956-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="b1956-122">Altri provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="b1956-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="b1956-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="b1956-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="b1956-124">L'app di esempio configura il provider di autenticazione Google con un ID client e un segreto client forniti da Google:</span><span class="sxs-lookup"><span data-stu-id="b1956-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="b1956-125">Definire l'ambito di autenticazione</span><span class="sxs-lookup"><span data-stu-id="b1956-125">Establish the authentication scope</span></span>

<span data-ttu-id="b1956-126">Specificare l'elenco di autorizzazioni da recuperare dal provider specificando la <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="b1956-127">Gli ambiti di autenticazione per i provider esterni comuni sono riportati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="b1956-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="b1956-128">Provider</span><span class="sxs-lookup"><span data-stu-id="b1956-128">Provider</span></span>  | <span data-ttu-id="b1956-129">Ambito</span><span class="sxs-lookup"><span data-stu-id="b1956-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="b1956-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="b1956-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="b1956-131">Google</span><span class="sxs-lookup"><span data-stu-id="b1956-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="b1956-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="b1956-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="b1956-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="b1956-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="b1956-134">Nell'app di esempio, l'ambito `userinfo.profile` di Google viene aggiunto automaticamente dal framework quando viene chiamato <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> sul <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b1956-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="b1956-135">Se l'app richiede ambiti aggiuntivi, aggiungerli alle opzioni.</span><span class="sxs-lookup"><span data-stu-id="b1956-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="b1956-136">Nell'esempio seguente viene aggiunto l'ambito di Google `https://www.googleapis.com/auth/user.birthday.read` per recuperare il compleanno di un utente:</span><span class="sxs-lookup"><span data-stu-id="b1956-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="b1956-137">Eseguire il mapping delle chiavi di dati utente e creare attestazioni</span><span class="sxs-lookup"><span data-stu-id="b1956-137">Map user data keys and create claims</span></span>

<span data-ttu-id="b1956-138">Nelle opzioni del provider specificare un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> per ogni chiave/sottochiave nei dati utente JSON del provider esterno per l'identità dell'app da leggere durante l'accesso.</span><span class="sxs-lookup"><span data-stu-id="b1956-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="b1956-139">Per ulteriori informazioni sui tipi di attestazione, vedere <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="b1956-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="b1956-140">L'app di esempio crea attestazioni delle impostazioni locali (`urn:google:locale`) e immagini (`urn:google:picture`) dalle chiavi di `locale` e `picture` nei dati utente di Google:</span><span class="sxs-lookup"><span data-stu-id="b1956-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="b1956-141">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) viene connesso all'app con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-141">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="b1956-142">Durante il processo di accesso, il <xref:Microsoft.AspNetCore.Identity.UserManager%601> può archiviare attestazioni `ApplicationUser` per i dati utente disponibili dal <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="b1956-143">Nell'app di esempio, `OnPostConfirmationAsync` (*account/ExternalLogin. cshtml. cs*) stabilisce le attestazioni delle impostazioni locali (`urn:google:locale`) e immagini (`urn:google:picture`) per la `ApplicationUser`con accesso, inclusa un'attestazione per <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="b1956-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="b1956-144">Per impostazione predefinita, le attestazioni di un utente vengono archiviate nel cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="b1956-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="b1956-145">Se il cookie di autenticazione è troppo grande, può causare un errore dell'app perché:</span><span class="sxs-lookup"><span data-stu-id="b1956-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="b1956-146">Il browser rileva che l'intestazione del cookie è troppo lungo.</span><span class="sxs-lookup"><span data-stu-id="b1956-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="b1956-147">La dimensione complessiva della richiesta è troppo grande.</span><span class="sxs-lookup"><span data-stu-id="b1956-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="b1956-148">Se per l'elaborazione delle richieste utente è richiesta una grande quantità di dati utente:</span><span class="sxs-lookup"><span data-stu-id="b1956-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="b1956-149">Limitare il numero e le dimensioni delle attestazioni utente per l'elaborazione delle richieste solo ai componenti richiesti dall'app.</span><span class="sxs-lookup"><span data-stu-id="b1956-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="b1956-150">Usare un <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> personalizzato per la <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> del middleware di autenticazione dei cookie per archiviare l'identità tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="b1956-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="b1956-151">Mantenere grandi quantità di informazioni sull'identità sul server inviando al client solo una chiave di identificatore di sessione di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="b1956-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="b1956-152">Salvare il token di accesso</span><span class="sxs-lookup"><span data-stu-id="b1956-152">Save the access token</span></span>

<span data-ttu-id="b1956-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> definisce se i token di accesso e di aggiornamento devono essere archiviati nel <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> dopo una corretta autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="b1956-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="b1956-154">per impostazione predefinita, `SaveTokens` è impostato su `false` per ridurre le dimensioni del cookie di autenticazione finale.</span><span class="sxs-lookup"><span data-stu-id="b1956-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="b1956-155">L'app di esempio imposta il valore di `SaveTokens` su `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="b1956-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="b1956-156">Quando `OnPostConfirmationAsync` esegue, archiviare il token di accesso ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dal provider esterno nel `AuthenticationProperties`del `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="b1956-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="b1956-157">L'app di esempio salva il token di accesso in `OnPostConfirmationAsync` (nuova registrazione utente) e `OnGetCallbackAsync` (utente registrato in precedenza) in *account/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="b1956-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="b1956-158">Come aggiungere token personalizzati aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="b1956-158">How to add additional custom tokens</span></span>

<span data-ttu-id="b1956-159">Per illustrare come aggiungere un token personalizzato, archiviato come parte di `SaveTokens`, l'app di esempio aggiunge una <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con il <xref:System.DateTime> corrente per un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) di `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="b1956-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="b1956-160">Creazione e aggiunta di attestazioni</span><span class="sxs-lookup"><span data-stu-id="b1956-160">Creating and adding claims</span></span>

<span data-ttu-id="b1956-161">Il Framework fornisce azioni comuni e metodi di estensione per la creazione e l'aggiunta di attestazioni alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="b1956-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="b1956-162">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> e <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="b1956-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="b1956-163">Gli utenti possono definire azioni personalizzate derivando da <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e implementando il metodo abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="b1956-164">Per ulteriori informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="b1956-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="b1956-165">Rimozione di azioni attestazioni e attestazioni</span><span class="sxs-lookup"><span data-stu-id="b1956-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="b1956-166">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) rimuove tutte le azioni di attestazione per il <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> specificato dalla raccolta.</span><span class="sxs-lookup"><span data-stu-id="b1956-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="b1956-167">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) Elimina un'attestazione del <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> specificato dall'identità.</span><span class="sxs-lookup"><span data-stu-id="b1956-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="b1956-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> viene utilizzata principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) per rimuovere le attestazioni generate dal protocollo.</span><span class="sxs-lookup"><span data-stu-id="b1956-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="b1956-169">Esempio di output dell'app</span><span class="sxs-lookup"><span data-stu-id="b1956-169">Sample app output</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1956-170">Un'app ASP.NET Core può creare attestazioni e token aggiuntivi da provider di autenticazione esterni, ad esempio Facebook, Google, Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="b1956-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="b1956-171">Ogni provider rivela informazioni diverse sugli utenti sulla propria piattaforma, ma il modello per la ricezione e la trasformazione dei dati utente in attestazioni aggiuntive è lo stesso.</span><span class="sxs-lookup"><span data-stu-id="b1956-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="b1956-172">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b1956-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1956-173">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b1956-173">Prerequisites</span></span>

<span data-ttu-id="b1956-174">Decidere quali provider di autenticazione esterni supportare nell'app.</span><span class="sxs-lookup"><span data-stu-id="b1956-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="b1956-175">Per ogni provider, registrare l'app e ottenere un ID client e un segreto client.</span><span class="sxs-lookup"><span data-stu-id="b1956-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="b1956-176">Per ulteriori informazioni, vedere <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="b1956-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="b1956-177">L'app di esempio usa il [provider di autenticazione Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="b1956-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="b1956-178">Impostare l'ID client e il segreto client</span><span class="sxs-lookup"><span data-stu-id="b1956-178">Set the client ID and client secret</span></span>

<span data-ttu-id="b1956-179">Il provider di autenticazione OAuth stabilisce una relazione di trust con un'app che usa un ID client e un segreto client.</span><span class="sxs-lookup"><span data-stu-id="b1956-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="b1956-180">I valori relativi all'ID client e al segreto client vengono creati per l'app dal provider di autenticazione esterno quando l'app viene registrata con il provider.</span><span class="sxs-lookup"><span data-stu-id="b1956-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="b1956-181">Ogni provider esterno usato dall'app deve essere configurato in modo indipendente con l'ID client e il segreto client del provider.</span><span class="sxs-lookup"><span data-stu-id="b1956-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="b1956-182">Per ulteriori informazioni, vedere gli argomenti del provider di autenticazione esterno applicabili allo scenario:</span><span class="sxs-lookup"><span data-stu-id="b1956-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="b1956-183">Autenticazione Facebook</span><span class="sxs-lookup"><span data-stu-id="b1956-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="b1956-184">Autenticazione Google</span><span class="sxs-lookup"><span data-stu-id="b1956-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="b1956-185">Autenticazione Microsoft</span><span class="sxs-lookup"><span data-stu-id="b1956-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="b1956-186">Autenticazione Twitter</span><span class="sxs-lookup"><span data-stu-id="b1956-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="b1956-187">Altri provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="b1956-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="b1956-188">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="b1956-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="b1956-189">L'app di esempio configura il provider di autenticazione Google con un ID client e un segreto client forniti da Google:</span><span class="sxs-lookup"><span data-stu-id="b1956-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="b1956-190">Definire l'ambito di autenticazione</span><span class="sxs-lookup"><span data-stu-id="b1956-190">Establish the authentication scope</span></span>

<span data-ttu-id="b1956-191">Specificare l'elenco di autorizzazioni da recuperare dal provider specificando la <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="b1956-192">Gli ambiti di autenticazione per i provider esterni comuni sono riportati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="b1956-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="b1956-193">Provider</span><span class="sxs-lookup"><span data-stu-id="b1956-193">Provider</span></span>  | <span data-ttu-id="b1956-194">Ambito</span><span class="sxs-lookup"><span data-stu-id="b1956-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="b1956-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="b1956-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="b1956-196">Google</span><span class="sxs-lookup"><span data-stu-id="b1956-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="b1956-197">Microsoft</span><span class="sxs-lookup"><span data-stu-id="b1956-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="b1956-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="b1956-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="b1956-199">Nell'app di esempio, l'ambito `userinfo.profile` di Google viene aggiunto automaticamente dal framework quando viene chiamato <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> sul <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b1956-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="b1956-200">Se l'app richiede ambiti aggiuntivi, aggiungerli alle opzioni.</span><span class="sxs-lookup"><span data-stu-id="b1956-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="b1956-201">Nell'esempio seguente viene aggiunto l'ambito di Google `https://www.googleapis.com/auth/user.birthday.read` per recuperare il compleanno di un utente:</span><span class="sxs-lookup"><span data-stu-id="b1956-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="b1956-202">Eseguire il mapping delle chiavi di dati utente e creare attestazioni</span><span class="sxs-lookup"><span data-stu-id="b1956-202">Map user data keys and create claims</span></span>

<span data-ttu-id="b1956-203">Nelle opzioni del provider specificare un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> per ogni chiave/sottochiave nei dati utente JSON del provider esterno per l'identità dell'app da leggere durante l'accesso.</span><span class="sxs-lookup"><span data-stu-id="b1956-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="b1956-204">Per ulteriori informazioni sui tipi di attestazione, vedere <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="b1956-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="b1956-205">L'app di esempio crea attestazioni delle impostazioni locali (`urn:google:locale`) e immagini (`urn:google:picture`) dalle chiavi di `locale` e `picture` nei dati utente di Google:</span><span class="sxs-lookup"><span data-stu-id="b1956-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="b1956-206">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) viene connesso all'app con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-206">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="b1956-207">Durante il processo di accesso, il <xref:Microsoft.AspNetCore.Identity.UserManager%601> può archiviare attestazioni `ApplicationUser` per i dati utente disponibili dal <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="b1956-208">Nell'app di esempio, `OnPostConfirmationAsync` (*account/ExternalLogin. cshtml. cs*) stabilisce le attestazioni delle impostazioni locali (`urn:google:locale`) e immagini (`urn:google:picture`) per la `ApplicationUser`con accesso, inclusa un'attestazione per <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="b1956-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="b1956-209">Per impostazione predefinita, le attestazioni di un utente vengono archiviate nel cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="b1956-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="b1956-210">Se il cookie di autenticazione è troppo grande, può causare un errore dell'app perché:</span><span class="sxs-lookup"><span data-stu-id="b1956-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="b1956-211">Il browser rileva che l'intestazione del cookie è troppo lungo.</span><span class="sxs-lookup"><span data-stu-id="b1956-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="b1956-212">La dimensione complessiva della richiesta è troppo grande.</span><span class="sxs-lookup"><span data-stu-id="b1956-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="b1956-213">Se per l'elaborazione delle richieste utente è richiesta una grande quantità di dati utente:</span><span class="sxs-lookup"><span data-stu-id="b1956-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="b1956-214">Limitare il numero e le dimensioni delle attestazioni utente per l'elaborazione delle richieste solo ai componenti richiesti dall'app.</span><span class="sxs-lookup"><span data-stu-id="b1956-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="b1956-215">Usare un <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> personalizzato per la <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> del middleware di autenticazione dei cookie per archiviare l'identità tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="b1956-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="b1956-216">Mantenere grandi quantità di informazioni sull'identità sul server inviando al client solo una chiave di identificatore di sessione di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="b1956-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="b1956-217">Salvare il token di accesso</span><span class="sxs-lookup"><span data-stu-id="b1956-217">Save the access token</span></span>

<span data-ttu-id="b1956-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> definisce se i token di accesso e di aggiornamento devono essere archiviati nel <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> dopo una corretta autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="b1956-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="b1956-219">per impostazione predefinita, `SaveTokens` è impostato su `false` per ridurre le dimensioni del cookie di autenticazione finale.</span><span class="sxs-lookup"><span data-stu-id="b1956-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="b1956-220">L'app di esempio imposta il valore di `SaveTokens` su `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="b1956-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="b1956-221">Quando `OnPostConfirmationAsync` esegue, archiviare il token di accesso ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dal provider esterno nel `AuthenticationProperties`del `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="b1956-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="b1956-222">L'app di esempio salva il token di accesso in `OnPostConfirmationAsync` (nuova registrazione utente) e `OnGetCallbackAsync` (utente registrato in precedenza) in *account/ExternalLogin. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="b1956-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="b1956-223">Come aggiungere token personalizzati aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="b1956-223">How to add additional custom tokens</span></span>

<span data-ttu-id="b1956-224">Per illustrare come aggiungere un token personalizzato, archiviato come parte di `SaveTokens`, l'app di esempio aggiunge una <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con il <xref:System.DateTime> corrente per un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) di `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="b1956-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="b1956-225">Creazione e aggiunta di attestazioni</span><span class="sxs-lookup"><span data-stu-id="b1956-225">Creating and adding claims</span></span>

<span data-ttu-id="b1956-226">Il Framework fornisce azioni comuni e metodi di estensione per la creazione e l'aggiunta di attestazioni alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="b1956-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="b1956-227">Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> e <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="b1956-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="b1956-228">Gli utenti possono definire azioni personalizzate derivando da <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e implementando il metodo abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="b1956-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="b1956-229">Per ulteriori informazioni, vedere <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="b1956-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="b1956-230">Rimozione di azioni attestazioni e attestazioni</span><span class="sxs-lookup"><span data-stu-id="b1956-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="b1956-231">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) rimuove tutte le azioni di attestazione per il <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> specificato dalla raccolta.</span><span class="sxs-lookup"><span data-stu-id="b1956-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="b1956-232">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) Elimina un'attestazione del <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> specificato dall'identità.</span><span class="sxs-lookup"><span data-stu-id="b1956-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="b1956-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> viene utilizzata principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) per rimuovere le attestazioni generate dal protocollo.</span><span class="sxs-lookup"><span data-stu-id="b1956-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="b1956-234">Esempio di output dell'app</span><span class="sxs-lookup"><span data-stu-id="b1956-234">Sample app output</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b1956-235">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b1956-235">Additional resources</span></span>

* <span data-ttu-id="b1956-236">[app SocialSample di ingegneria DotNet/AspNetCore](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l'app di esempio collegata si trova nel ramo di progettazione della `master` del [repository GitHub DotNet/AspNetCore](https://github.com/dotnet/AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="b1956-236">[dotnet/AspNetCore engineering SocialSample app](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [dotnet/AspNetCore GitHub repo's](https://github.com/dotnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="b1956-237">Il ramo `master` contiene il codice in fase di sviluppo attivo per la versione successiva di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1956-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="b1956-238">Per visualizzare una versione dell'app di esempio per una versione rilasciata di ASP.NET Core, usare l'elenco a discesa **Branch** per selezionare un ramo di rilascio, ad esempio `release/{X.Y}`.</span><span class="sxs-lookup"><span data-stu-id="b1956-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
