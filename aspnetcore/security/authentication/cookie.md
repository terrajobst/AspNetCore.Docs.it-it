---
title: Utilizzare l'autenticazione dei cookie senza ASP.NET Identity Core
author: rick-anderson
description: Una spiegazione dell'utilizzo dell'autenticazione cookie senza ASP.NET Identity Core
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 82f826bbc2bb19339851d5e25c539ea2c03acfb3
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819110"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="dc34b-103">Utilizzare l'autenticazione dei cookie senza ASP.NET Identity Core</span><span class="sxs-lookup"><span data-stu-id="dc34b-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="dc34b-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dc34b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dc34b-105">Come si è visto negli argomenti precedenti autenticazione [ASP.NET Identity Core](xref:security/authentication/identity) è un provider di autenticazione completa e completa per creare e gestire gli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="dc34b-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="dc34b-106">Tuttavia, si consiglia di utilizzare la logica di autenticazione personalizzato con l'autenticazione basata su cookie in alcuni casi.</span><span class="sxs-lookup"><span data-stu-id="dc34b-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="dc34b-107">È possibile utilizzare l'autenticazione basata su cookie come un provider di autenticazione autonomo senza ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="dc34b-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="dc34b-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc34b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dc34b-109">A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="dc34b-110">Utilizzare il nome utente di posta elettronica "maria.rodriguez@contoso.com" e indicare una password di accesso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="dc34b-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="dc34b-111">L'utente viene autenticato nel `AuthenticateUser` metodo il *Pages/Account/Login.cshtml.cs* file.</span><span class="sxs-lookup"><span data-stu-id="dc34b-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="dc34b-112">In un esempio reale, l'utente viene autenticato un database.</span><span class="sxs-lookup"><span data-stu-id="dc34b-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="dc34b-113">Per informazioni sulla migrazione l'autenticazione basata su cookie da ASP.NET Core 1.x a 2.0, vedere [eseguire la migrazione di autenticazione e identità all'argomento di ASP.NET Core 2.0 (basato su Cookie di autenticazione)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="dc34b-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="dc34b-114">Per utilizzare ASP.NET Identity Core, vedere la [Introduzione all'identità](xref:security/authentication/identity) argomento.</span><span class="sxs-lookup"><span data-stu-id="dc34b-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="dc34b-115">Configurazione</span><span class="sxs-lookup"><span data-stu-id="dc34b-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc34b-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dc34b-117">Se l'app non usa il [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto (versione 2.1.0 o in un secondo momento).</span><span class="sxs-lookup"><span data-stu-id="dc34b-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="dc34b-118">Nel `ConfigureServices` (metodo), creare il servizio di Middleware di autenticazione con il `AddAuthentication` e `AddCookie` metodi:</span><span class="sxs-lookup"><span data-stu-id="dc34b-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="dc34b-119">`AuthenticationScheme` passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="dc34b-120">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzazione con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="dc34b-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="dc34b-121">L'impostazione di `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore pari a "Cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="dc34b-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="dc34b-122">È possibile fornire qualsiasi valore stringa che distingue lo schema.</span><span class="sxs-lookup"><span data-stu-id="dc34b-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="dc34b-123">Nel `Configure` metodo, utilizzare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che imposta il `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc34b-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="dc34b-124">Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="dc34b-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="dc34b-125">**Opzioni AddCookie**</span><span class="sxs-lookup"><span data-stu-id="dc34b-125">**AddCookie Options**</span></span>

<span data-ttu-id="dc34b-126">Il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="dc34b-127">Opzione</span><span class="sxs-lookup"><span data-stu-id="dc34b-127">Option</span></span> | <span data-ttu-id="dc34b-128">Descrizione</span><span class="sxs-lookup"><span data-stu-id="dc34b-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="dc34b-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="dc34b-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-130">Fornisce il percorso di fornire un 302 trovato (reindirizzamento dell'URL) quando sono attivati da `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="dc34b-131">Il valore predefinito è `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="dc34b-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="dc34b-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-133">L'autorità emittente da utilizzare per il [dell'autorità di certificazione](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creata dal servizio di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="dc34b-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="dc34b-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-135">Il nome di dominio in cui il cookie viene servito.</span><span class="sxs-lookup"><span data-stu-id="dc34b-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="dc34b-136">Per impostazione predefinita, questo è il nome host della richiesta.</span><span class="sxs-lookup"><span data-stu-id="dc34b-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="dc34b-137">Il browser invia solo il cookie nelle richieste inviate a un nome host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="dc34b-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="dc34b-138">Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio.</span><span class="sxs-lookup"><span data-stu-id="dc34b-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="dc34b-139">Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="dc34b-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="dc34b-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-141">Ottiene o imposta la durata di un cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="dc34b-142">Attualmente, questa opzione no ops e diventerà obsoleta in ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="dc34b-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="dc34b-143">Utilizzare il `ExpireTimeSpan` opzione per impostare la scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="dc34b-144">Per ulteriori informazioni, vedere [chiarire il comportamento di CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="dc34b-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="dc34b-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="dc34b-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-146">Flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="dc34b-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="dc34b-147">Modifica questo valore su `false` consenta script lato client per accedere al cookie e possono aprire l'app al rischio di furto di cookie che l'app deve avere un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità.</span><span class="sxs-lookup"><span data-stu-id="dc34b-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="dc34b-148">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="dc34b-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="dc34b-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-150">Imposta il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="dc34b-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="dc34b-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-152">Utilizzato per isolare le app eseguono lo stesso nome host.</span><span class="sxs-lookup"><span data-stu-id="dc34b-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="dc34b-153">Se si dispone di un'app in esecuzione in `/app1` e desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="dc34b-154">In questo modo, il cookie è disponibile solo per le richieste per `/app1` e tutte le app disponibili al suo interno.</span><span class="sxs-lookup"><span data-stu-id="dc34b-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="dc34b-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="dc34b-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-156">Indica se il browser deve consentire i cookie da collegare alle richieste di nello stesso sito solo (`SameSiteMode.Strict`) o le richieste tra siti tramite i metodi HTTP sicuro e richieste di nello stesso sito (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="dc34b-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="dc34b-157">Se impostato su `SameSiteMode.None`, non è impostato il valore dell'intestazione cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="dc34b-158">Si noti che [Middleware criteri Cookie](#cookie-policy-middleware) potrebbe sovrascrivere il valore fornito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="dc34b-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="dc34b-159">Per supportare l'autenticazione OAuth, il valore predefinito è `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="dc34b-160">Per ulteriori informazioni, vedere [autenticazione OAuth interrotto a causa dei criteri di cookie SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="dc34b-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="dc34b-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="dc34b-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-162">Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="dc34b-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="dc34b-163">Il valore predefinito è `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="dc34b-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="dc34b-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-165">Imposta il `DataProtectionProvider` che viene utilizzato per creare il valore predefinito `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="dc34b-166">Se il `TicketDataFormat` è impostata, il `DataProtectionProvider` non viene usata l'opzione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="dc34b-167">Se omesso, viene utilizzato provider di protezione dati predefinito dell'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="dc34b-168">Eventi</span><span class="sxs-lookup"><span data-stu-id="dc34b-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-169">Il gestore chiama i metodi nel provider che offrono il controllo di app in alcune fasi di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="dc34b-170">Se `Events` non fornito, viene fornita un'istanza predefinita che non esegue alcuna operazione quando i metodi vengono chiamati.</span><span class="sxs-lookup"><span data-stu-id="dc34b-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="dc34b-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="dc34b-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-172">Utilizzato come tipo di servizio per ottenere il `Events` istanza anziché la proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc34b-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="dc34b-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="dc34b-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-174">Il `TimeSpan` dopo che il ticket di autenticazione archiviato il cookie scade.</span><span class="sxs-lookup"><span data-stu-id="dc34b-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="dc34b-175">`ExpireTimeSpan` viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket.</span><span class="sxs-lookup"><span data-stu-id="dc34b-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="dc34b-176">Il `ExpiredTimeSpan` valore diventa sempre il ticket di autenticazione crittografato verificati dal server.</span><span class="sxs-lookup"><span data-stu-id="dc34b-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="dc34b-177">Può inoltre analizzare le [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) intestazione, ma solo se `IsPersistent` è impostata.</span><span class="sxs-lookup"><span data-stu-id="dc34b-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="dc34b-178">Per impostare `IsPersistent` a `true`, configurare il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passato a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="dc34b-179">Il valore predefinito di `ExpireTimeSpan` è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="dc34b-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="dc34b-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="dc34b-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-181">Fornisce il percorso di fornire un 302 trovato (reindirizzamento dell'URL) quando sono attivati da `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="dc34b-182">L'URL corrente che ha generato il codice 401 viene aggiunto per il `LoginPath` come parametro di stringa di query denominato dal `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="dc34b-183">Una volta una richiesta per il `LoginPath` concede una nuova identità di accesso, il `ReturnUrlParameter` valore viene utilizzato per reindirizzare il browser all'URL che ha causato il codice di stato unauthorized originale.</span><span class="sxs-lookup"><span data-stu-id="dc34b-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="dc34b-184">Il valore predefinito è `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="dc34b-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="dc34b-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-186">Se il `LogoutPath` viene fornito per il gestore, quindi reindirizza una richiesta per il percorso in base al valore del `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="dc34b-187">Il valore predefinito è `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="dc34b-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="dc34b-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-189">Determina il nome del parametro della stringa di query che viene aggiunto dal gestore di risposta 302 trovato (reindirizzamento dell'URL).</span><span class="sxs-lookup"><span data-stu-id="dc34b-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="dc34b-190">`ReturnUrlParameter` viene utilizzato quando arriva una richiesta di `LoginPath` o `LogoutPath` per restituire il browser all'URL originale al termine dell'operazione di connessione o disconnessione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="dc34b-191">Il valore predefinito è `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="dc34b-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="dc34b-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-193">Un contenitore facoltativo utilizzato per archiviare l'identità in tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="dc34b-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="dc34b-194">Se utilizzato, solo un identificatore di sessione viene inviato al client.</span><span class="sxs-lookup"><span data-stu-id="dc34b-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="dc34b-195">`SessionStore` Consente di attenuare i potenziali problemi con le identità di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="dc34b-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="dc34b-196">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="dc34b-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-197">Un flag che indica se un nuovo cookie con una scadenza aggiornato deve essere visualizzato in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="dc34b-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="dc34b-198">Questa situazione può verificarsi in qualsiasi richiesta in cui il periodo di scadenza cookie corrente è superiore al 50% scaduto.</span><span class="sxs-lookup"><span data-stu-id="dc34b-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="dc34b-199">La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="dc34b-200">Un [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="dc34b-201">Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando il periodo di tempo che il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="dc34b-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="dc34b-202">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="dc34b-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="dc34b-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-204">Il `TicketDataFormat` viene utilizzata per proteggere e annullare la protezione dell'identità e altre proprietà che vengono archiviate nel valore del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="dc34b-205">Se non specificato, un `TicketDataFormat` viene creato utilizzando il [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="dc34b-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="dc34b-206">Convalidare</span><span class="sxs-lookup"><span data-stu-id="dc34b-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="dc34b-207">Metodo che verifica che le opzioni sono valide.</span><span class="sxs-lookup"><span data-stu-id="dc34b-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="dc34b-208">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="dc34b-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc34b-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dc34b-210">ASP.NET Core 1. x utilizza cookie [middleware](xref:fundamentals/middleware/index) che serializza un'entità utente in un cookie crittografato.</span><span class="sxs-lookup"><span data-stu-id="dc34b-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="dc34b-211">Nelle richieste successive, il cookie viene convalidato e ricreato e assegnata l'entità di `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc34b-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="dc34b-212">Installare il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto NuGet nel progetto.</span><span class="sxs-lookup"><span data-stu-id="dc34b-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="dc34b-213">Questo pacchetto contiene middleware del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="dc34b-214">Utilizzare il `UseCookieAuthentication` metodo nel `Configure` metodo i *Startup.cs* file prima di `UseMvc` o `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="dc34b-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="dc34b-215">**Opzioni di CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="dc34b-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="dc34b-216">Il [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="dc34b-217">Opzione</span><span class="sxs-lookup"><span data-stu-id="dc34b-217">Option</span></span> | <span data-ttu-id="dc34b-218">Descrizione</span><span class="sxs-lookup"><span data-stu-id="dc34b-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="dc34b-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="dc34b-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-220">Imposta lo schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-220">Sets the authentication scheme.</span></span> <span data-ttu-id="dc34b-221">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione e si desidera [autorizzazione con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="dc34b-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="dc34b-222">L'impostazione di `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore pari a "Cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="dc34b-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="dc34b-223">È possibile fornire qualsiasi valore stringa che distingue lo schema.</span><span class="sxs-lookup"><span data-stu-id="dc34b-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="dc34b-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="dc34b-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-225">Imposta un valore per indicare che l'autenticazione dei cookie di eseguire in ogni richiesta e tenta di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="dc34b-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="dc34b-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="dc34b-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-227">Se true, il middleware di autenticazione gestisce sfide automatico.</span><span class="sxs-lookup"><span data-stu-id="dc34b-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="dc34b-228">Se false, il middleware di autenticazione altera solo le risposte quando richiesto esplicitamente dal `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="dc34b-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="dc34b-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-230">L'autorità emittente da utilizzare per il [dell'autorità di certificazione](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal middleware di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="dc34b-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="dc34b-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-232">Il nome di dominio in cui il cookie viene servito.</span><span class="sxs-lookup"><span data-stu-id="dc34b-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="dc34b-233">Per impostazione predefinita, questo è il nome host della richiesta.</span><span class="sxs-lookup"><span data-stu-id="dc34b-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="dc34b-234">Il browser opera solo il cookie a un nome host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="dc34b-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="dc34b-235">Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio.</span><span class="sxs-lookup"><span data-stu-id="dc34b-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="dc34b-236">Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="dc34b-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="dc34b-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-238">Flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="dc34b-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="dc34b-239">Modifica questo valore su `false` consenta script lato client per accedere al cookie e possono aprire l'app al rischio di furto di cookie che l'app deve avere un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità.</span><span class="sxs-lookup"><span data-stu-id="dc34b-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="dc34b-240">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="dc34b-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="dc34b-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-242">Utilizzato per isolare le app eseguono lo stesso nome host.</span><span class="sxs-lookup"><span data-stu-id="dc34b-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="dc34b-243">Se si dispone di un'app in esecuzione in `/app1` e desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="dc34b-244">In questo modo, il cookie è disponibile solo per le richieste per `/app1` e tutte le app disponibili al suo interno.</span><span class="sxs-lookup"><span data-stu-id="dc34b-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="dc34b-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="dc34b-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-246">Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="dc34b-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="dc34b-247">Il valore predefinito è `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="dc34b-248">Descrizione</span><span class="sxs-lookup"><span data-stu-id="dc34b-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-249">Informazioni aggiuntive sul tipo di autenticazione viene resa disponibile per l'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="dc34b-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="dc34b-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-251">Il `TimeSpan` dopo cui scade il ticket di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="dc34b-252">Viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket.</span><span class="sxs-lookup"><span data-stu-id="dc34b-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="dc34b-253">Per utilizzare `ExpireTimeSpan`, è necessario impostare `IsPersistent` a `true` nel `AuthenticationProperties` passato a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="dc34b-254">Il valore predefinito è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="dc34b-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="dc34b-255">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="dc34b-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="dc34b-256">Un flag che indica se la data di scadenza del cookie viene reimpostato quando più di semestre di `ExpireTimeSpan` trascorso l'intervallo.</span><span class="sxs-lookup"><span data-stu-id="dc34b-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="dc34b-257">La nuova ora exipiration viene spostata in avanti la data corrente più il `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="dc34b-258">Un [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="dc34b-259">Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando il periodo di tempo che il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="dc34b-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="dc34b-260">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-260">The default value is `true`.</span></span> |

<span data-ttu-id="dc34b-261">Impostare `CookieAuthenticationOptions` per il Middleware di autenticazione nel Cookie di `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="dc34b-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="dc34b-262">Middleware criteri cookie</span><span class="sxs-lookup"><span data-stu-id="dc34b-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="dc34b-263">[Cookie criteri Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) consente le funzionalità di criteri di cookie in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="dc34b-264">L'aggiunta di middleware alla pipeline di elaborazione app è ordine sensibili; riguarda solo i componenti registrati dopo di esso nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="dc34b-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="dc34b-265">Il [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fornito per il Middleware di criteri di Cookie consentono di controllare caratteristiche globali del cookie elaborazione e l'hook in elaborazione cookie gestori quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="dc34b-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="dc34b-266">Proprietà</span><span class="sxs-lookup"><span data-stu-id="dc34b-266">Property</span></span> | <span data-ttu-id="dc34b-267">Descrizione</span><span class="sxs-lookup"><span data-stu-id="dc34b-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="dc34b-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="dc34b-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="dc34b-269">Interessa se i cookie devono essere HttpOnly, che è un flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="dc34b-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="dc34b-270">Il valore predefinito è `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="dc34b-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="dc34b-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="dc34b-272">Interessa attributo nello stesso sito del cookie (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="dc34b-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="dc34b-273">Il valore predefinito è `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="dc34b-274">Questa opzione è disponibile per i componenti di base di ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="dc34b-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="dc34b-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="dc34b-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="dc34b-276">Chiamato quando viene aggiunto un cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="dc34b-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="dc34b-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="dc34b-278">Chiamato quando viene eliminato un cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="dc34b-279">Proteggere</span><span class="sxs-lookup"><span data-stu-id="dc34b-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="dc34b-280">Determina se i cookie devono essere protetto.</span><span class="sxs-lookup"><span data-stu-id="dc34b-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="dc34b-281">Il valore predefinito è `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="dc34b-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + solo)</span><span class="sxs-lookup"><span data-stu-id="dc34b-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="dc34b-283">Il valore predefinito `MinimumSameSitePolicy` valore `SameSiteMode.Lax` per consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="dc34b-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="dc34b-284">Per applicare rigorosamente criteri nello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="dc34b-285">Anche se questa impostazione di interruzioni di OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di App che non richiedono l'elaborazione della richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="dc34b-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="dc34b-286">L'impostazione di Middleware di criteri di Cookie per `MinimumSameSitePolicy` possono influenzare l'impostazione di `Cookie.SameSite` in `CookieAuthenticationOptions` impostazioni in base alla matrice di seguito.</span><span class="sxs-lookup"><span data-stu-id="dc34b-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="dc34b-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="dc34b-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="dc34b-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="dc34b-288">Cookie.SameSite</span></span> | <span data-ttu-id="dc34b-289">Impostazione Cookie.SameSite risultante</span><span class="sxs-lookup"><span data-stu-id="dc34b-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="dc34b-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="dc34b-290">SameSiteMode.None</span></span>     | <span data-ttu-id="dc34b-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="dc34b-291">SameSiteMode.None</span></span><br><span data-ttu-id="dc34b-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="dc34b-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="dc34b-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="dc34b-294">SameSiteMode.None</span></span><br><span data-ttu-id="dc34b-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="dc34b-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="dc34b-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="dc34b-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="dc34b-298">SameSiteMode.None</span></span><br><span data-ttu-id="dc34b-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="dc34b-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="dc34b-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="dc34b-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="dc34b-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="dc34b-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="dc34b-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="dc34b-305">SameSiteMode.None</span></span><br><span data-ttu-id="dc34b-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="dc34b-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="dc34b-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="dc34b-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="dc34b-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="dc34b-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="dc34b-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="dc34b-311">Crea un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="dc34b-311">Create an authentication cookie</span></span>

<span data-ttu-id="dc34b-312">Per creare un cookie contenente le informazioni sull'utente, è necessario costruire un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="dc34b-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="dc34b-313">Le informazioni utente vengano serializzate e memorizzate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc34b-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dc34b-315">Creare un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con le necessarie [attestazione](/dotnet/api/system.security.claims.claim)s e chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) per l'accesso dell'utente:</span><span class="sxs-lookup"><span data-stu-id="dc34b-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc34b-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dc34b-317">Chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) per l'accesso dell'utente:</span><span class="sxs-lookup"><span data-stu-id="dc34b-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="dc34b-318">`SignInAsync` Crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="dc34b-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="dc34b-319">Se non si specifica un `AuthenticationScheme`, viene utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="dc34b-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="dc34b-320">Dietro le quinte, la crittografia utilizzata è ASP.NET Core [la protezione dei dati](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="dc34b-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="dc34b-321">Se si ospita l'applicazione in più computer, il bilanciamento del carico tra App o utilizzando una web farm, quindi è necessario [configurare la protezione dati](xref:security/data-protection/configuration/overview) per utilizzare la stessa chiave anello e identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="dc34b-322">Esci</span><span class="sxs-lookup"><span data-stu-id="dc34b-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc34b-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dc34b-324">Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="dc34b-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc34b-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dc34b-326">Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="dc34b-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="dc34b-327">Se non si utilizza `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookie") per lo schema (ad esempio, "ContosoCookie"), specificare lo schema usato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="dc34b-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="dc34b-328">In caso contrario, viene utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="dc34b-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="dc34b-329">Rispondere alle modifiche di back-end</span><span class="sxs-lookup"><span data-stu-id="dc34b-329">React to back-end changes</span></span>

<span data-ttu-id="dc34b-330">Dopo aver creato un cookie, diventa l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="dc34b-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="dc34b-331">Anche se si disabilita un utente nei sistemi back-end, il sistema di autenticazione cookie non ha alcuna conoscenza di questo oggetto e un utente rimane connesso, purché i cookie è valido.</span><span class="sxs-lookup"><span data-stu-id="dc34b-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="dc34b-332">Il [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) evento in ASP.NET Core 2. x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodo in ASP.NET Core 1. x consente di intercettare ed eseguire l'override di convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="dc34b-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="dc34b-333">Questo approccio riduce il rischio di utenti revocati l'accesso all'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="dc34b-334">Un approccio alla convalida dei cookie è basato su tenere traccia di quando il database utente è stato modificato.</span><span class="sxs-lookup"><span data-stu-id="dc34b-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="dc34b-335">Se il database non è stato modificato perché è stato emesso il cookie dell'utente, non è necessario autenticare nuovamente l'utente se il cookie è ancora valido.</span><span class="sxs-lookup"><span data-stu-id="dc34b-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="dc34b-336">Implementare questo scenario, il database, che viene implementato in `IUserRepository` per questo esempio, archivia un `LastChanged` valore.</span><span class="sxs-lookup"><span data-stu-id="dc34b-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="dc34b-337">Quando un utente viene aggiornato nel database, il `LastChanged` è impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="dc34b-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="dc34b-338">Per invalidare un cookie quando le modifiche del database basano sul `LastChanged` valore, creare il cookie con una `LastChanged` attestazione contenente corrente `LastChanged` valore dal database:</span><span class="sxs-lookup"><span data-stu-id="dc34b-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc34b-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dc34b-340">Per implementare un override per il `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe derivata da [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="dc34b-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="dc34b-341">Un esempio è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dc34b-341">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="dc34b-342">Registrare l'istanza di eventi durante la registrazione del servizio nel cookie di `ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="dc34b-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="dc34b-343">Fornire una registrazione di servizio con ambito per il `CustomCookieAuthenticationEvents` classe:</span><span class="sxs-lookup"><span data-stu-id="dc34b-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc34b-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dc34b-345">Per implementare un override per il `ValidateAsync` eventi, scrivere un metodo con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="dc34b-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="dc34b-346">ASP.NET Identity Core implementa questo controllo come parte del relativo [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="dc34b-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="dc34b-347">Un esempio è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dc34b-347">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="dc34b-348">Registrare l'evento durante la configurazione dell'autenticazione nel cookie di `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="dc34b-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="dc34b-349">Si consideri una situazione in cui viene aggiornato il nome dell'utente &mdash; una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="dc34b-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="dc34b-350">Se si desidera aggiornare distruttivo l'entità utente, chiamare `context.ReplacePrincipal` e impostare il `context.ShouldRenew` proprietà `true`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="dc34b-351">L'approccio descritto di seguito viene attivata ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="dc34b-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="dc34b-352">Ciò può comportare una riduzione delle prestazioni di grandi dimensioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="dc34b-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="dc34b-353">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="dc34b-353">Persistent cookies</span></span>

<span data-ttu-id="dc34b-354">È possibile che il cookie per rendere persistenti le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="dc34b-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="dc34b-355">Questo tipo di persistenza deve essere abilitata solo con il consenso esplicito utente con un controllo checkbox "Memorizza dati" su account di accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="dc34b-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="dc34b-356">Il seguente frammento di codice crea un'identità e i cookie corrispondente che resta tramite chiusure browser.</span><span class="sxs-lookup"><span data-stu-id="dc34b-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="dc34b-357">Le impostazioni di scadenza variabile configurate in precedenza vengono rispettate.</span><span class="sxs-lookup"><span data-stu-id="dc34b-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="dc34b-358">Se il cookie scade mentre il browser viene chiuso, il browser Cancella cookie quando viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="dc34b-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc34b-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="dc34b-360">Il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) risiede nella classe di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="dc34b-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc34b-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="dc34b-362">Il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) risiede nella classe di `Microsoft.AspNetCore.Http.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="dc34b-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="dc34b-363">Scadenza del cookie assoluto</span><span class="sxs-lookup"><span data-stu-id="dc34b-363">Absolute cookie expiration</span></span>

<span data-ttu-id="dc34b-364">È possibile impostare un'ora di scadenza assoluto con `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="dc34b-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="dc34b-365">È inoltre necessario impostare `IsPersistent`; in caso contrario, `ExpiresUtc` viene ignorato e viene creato un cookie di sessione singola.</span><span class="sxs-lookup"><span data-stu-id="dc34b-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="dc34b-366">Quando `ExpiresUtc` è impostato su `SignInAsync`, sostituisce il valore del `ExpireTimeSpan` opzione di `CookieAuthenticationOptions`, se impostata.</span><span class="sxs-lookup"><span data-stu-id="dc34b-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="dc34b-367">Il seguente frammento di codice crea un'identità e i cookie corrispondente che la durata è di 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="dc34b-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="dc34b-368">Questo ignora eventuali impostazioni di scadenza variabile configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="dc34b-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dc34b-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dc34b-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dc34b-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="additional-resources"></a><span data-ttu-id="dc34b-371">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dc34b-371">Additional resources</span></span>

* [<span data-ttu-id="dc34b-372">Modifiche di autenticazione 2.0 / migrazione annuncio</span><span class="sxs-lookup"><span data-stu-id="dc34b-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="dc34b-373">Limitare l'identità tramite schema</span><span class="sxs-lookup"><span data-stu-id="dc34b-373">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="dc34b-374">Autorizzazione basata sulle attestazioni</span><span class="sxs-lookup"><span data-stu-id="dc34b-374">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="dc34b-375">Controlli del ruolo basata su criteri</span><span class="sxs-lookup"><span data-stu-id="dc34b-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
