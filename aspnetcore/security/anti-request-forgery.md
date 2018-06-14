---
title: Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core
author: steve-smith
description: Informazioni su come prevenire gli attacchi contro le app web in un sito Web dannoso può influenzare l'interazione tra un browser del client e l'app.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341860"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="f7e5e-103">Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7e5e-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="f7e5e-104">Dal [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f7e5e-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f7e5e-105">Falsificazione della richiesta tra siti (noto anche come XSRF o CSRF, accentuato *tra superfici vedere*) è un attacco contro App web ospitate in base al quale un'app web dannoso può influenzare l'interazione tra un browser del client e un'app web che considera attendibile che Browser.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="f7e5e-106">Questi attacchi sono possibili in quanto i web browser invia alcuni tipi di token di autenticazione automaticamente con tutte le richieste a un sito Web.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="f7e5e-107">Questa forma di attacco è noto anche come un *con un clic attacco* oppure *sessione da* perché l'attacco sfrutta l'utente autenticato del precedentemente sessione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="f7e5e-108">Un esempio di un attacco di tipo CSRF:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="f7e5e-109">Un utente accede a `www.good-banking-site.com` tramite autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="f7e5e-110">Il server autentica l'utente e invia una risposta che include un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="f7e5e-111">Il sito è vulnerabile agli attacchi poiché si affida qualsiasi richiesta che riceve con un cookie di autenticazione valido.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="f7e5e-112">L'utente visita un sito dannoso, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="f7e5e-113">Il sito dannoso, `www.bad-crook-site.com`, contiene un form HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="f7e5e-114">Si noti che il modulo `action` post per il sito vulnerabile, non per il sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="f7e5e-115">Questa è la parte "cross-site" di CSRF.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="f7e5e-116">L'utente seleziona il pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-116">The user selects the submit button.</span></span> <span data-ttu-id="f7e5e-117">Il browser effettua la richiesta e include automaticamente il cookie di autenticazione per il dominio richiesto `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="f7e5e-118">La richiesta viene eseguita nel `www.good-banking-site.com` server con il contesto di autenticazione dell'utente e possono eseguire qualsiasi azione che un utente autenticato è autorizzato a eseguire.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="f7e5e-119">Oltre a uno scenario in cui l'utente seleziona il pulsante di invio del form, il sito dannoso è stato possibile:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="f7e5e-120">Eseguire uno script che invia automaticamente il form.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="f7e5e-121">Inviare l'invio del modulo come una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="f7e5e-122">Nascondere il form utilizzando CSS.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-122">Hide the form using CSS.</span></span>

<span data-ttu-id="f7e5e-123">Questi scenari alternativi non richiedono un'azione o l'input dell'utente diverso da inizialmente visitare il sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="f7e5e-124">L'uso di HTTPS non impedire un attacco di tipo CSRF.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="f7e5e-125">Il sito dannoso può inviare un `https://www.good-banking-site.com/` richiedere la stessa facilità con cui può inviare una richiesta non protetta.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="f7e5e-126">Alcuni attacchi di destinazione che rispondono alle richieste GET, nel qual caso un tag di immagine può essere utilizzato per eseguire l'azione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="f7e5e-127">Questa forma di attacco è comune nei siti di forum che consentono le immagini ma bloccano JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="f7e5e-128">Le app che modificano lo stato per le richieste GET, in cui le variabili o le risorse vengono modificate, sono vulnerabili ad attacchi dannosi.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="f7e5e-129">**Le richieste GET che modificano lo stato sono non protette. Una procedura consigliata consiste nel non modificare mai stato in una richiesta GET.**</span><span class="sxs-lookup"><span data-stu-id="f7e5e-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="f7e5e-130">Attacchi CSRF sono possibili con le app web che usano i cookie per l'autenticazione perché:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="f7e5e-131">Browser memorizzano i cookie emessi da un'app web.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="f7e5e-132">Cookie stored includono i cookie di sessione per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="f7e5e-133">Browser inviano che tutti i cookie associati a un dominio all'app web tutte le richieste indipendentemente dal modo in cui è stata generata la richiesta all'app all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="f7e5e-134">Tuttavia, non sono limitati CSRF attacchi per sfruttare i cookie.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="f7e5e-135">Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="f7e5e-136">Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali finché la sessione&dagger; termina.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="f7e5e-137">&dagger;In questo contesto *sessione* fa riferimento alla sessione dal lato client durante i quali l'autenticazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="f7e5e-138">È non correlati alle sessioni sul lato server o [ASP.NET Core sessione Middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="f7e5e-139">Gli utenti possono proteggersi dal vulnerabilità CSRF adottando delle precauzioni:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="f7e5e-140">Firma dell'App web al termine di usarli.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="f7e5e-141">Periodicamente i cookie del browser non crittografato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="f7e5e-142">Tuttavia, le vulnerabilità CSRF sono fondamentalmente un problema dell'app web, non dell'utente finale.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="f7e5e-143">Nozioni fondamentali di autenticazione</span><span class="sxs-lookup"><span data-stu-id="f7e5e-143">Authentication fundamentals</span></span>

<span data-ttu-id="f7e5e-144">L'autenticazione basata su cookie è una forma comune di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="f7e5e-145">Sistemi di autenticazione basata su token sono sempre più diffusi, soprattutto per applicazioni a pagina singola (SPAs).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="f7e5e-146">Autenticazione basata su cookie</span><span class="sxs-lookup"><span data-stu-id="f7e5e-146">Cookie-based authentication</span></span>

<span data-ttu-id="f7e5e-147">Quando un utente viene autenticato mediante il nome utente e password, che sta emessi un token contenente un ticket di autenticazione che può essere utilizzato per l'autenticazione e autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="f7e5e-148">Il token viene memorizzato come rende un cookie associato a ogni richiesta client.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="f7e5e-149">Generazione e la convalida di questo cookie viene eseguita dal Middleware del Cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="f7e5e-150">Il [middleware](xref:fundamentals/middleware/index) serializza un'entità utente in un cookie crittografato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="f7e5e-151">Nelle richieste successive, il middleware convalida il cookie, ricrea l'entità e assegna all'entità del [utente](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) proprietà di [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="f7e5e-152">Autenticazione basata su token</span><span class="sxs-lookup"><span data-stu-id="f7e5e-152">Token-based authentication</span></span>

<span data-ttu-id="f7e5e-153">Quando un utente viene autenticato, essi sta rilasciati un token (non un token non riproducibili).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="f7e5e-154">Il token contiene le informazioni utente nel formato [attestazioni](/dotnet/framework/security/claims-based-identity-model) o un token di riferimento che punta all'app lo stato utente gestito nell'app.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="f7e5e-155">Quando un utente tenta di accedere a una risorsa che richiede l'autenticazione, il token viene inviato all'app con un'intestazione di autorizzazione aggiuntive sotto forma di token di connessione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="f7e5e-156">In questo modo l'app senza stato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-156">This makes the app stateless.</span></span> <span data-ttu-id="f7e5e-157">In tutte le richieste successive, il token viene passato nella richiesta per la convalida lato server.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="f7e5e-158">Questo token non è *crittografati*; ha *codificato*.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="f7e5e-159">Nel server, il token viene decodificato per accedere alle relative informazioni.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="f7e5e-160">Per inviare il token nelle richieste successive, memorizzare il token nel servizio di archiviazione locale del browser.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="f7e5e-161">Non occorre preoccuparsi sulle vulnerabilità CSRF se il token viene memorizzato nell'archivio locale del browser.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="f7e5e-162">CSRF costituisce un problema quando il token viene archiviato in un cookie.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="f7e5e-163">Più App ospitate in un dominio</span><span class="sxs-lookup"><span data-stu-id="f7e5e-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="f7e5e-164">Ambienti di hosting condivisi sono vulnerabili a Hijack della sessione, account di accesso CSRF e altri attacchi.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="f7e5e-165">Sebbene `example1.contoso.net` e `example2.contoso.net` sono host diversi, c'è una relazione di trust implicita tra gli host sotto il `*.contoso.net` dominio.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="f7e5e-166">Questa relazione di trust implicita consente agli host potenzialmente non attendibili determinare i cookie di altro (lo stessa origine i criteri che regolano le richieste AJAX non applicano necessariamente ai cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="f7e5e-167">Per impedire gli attacchi che sfruttano i cookie attendibili tra le applicazioni ospitate nello stesso dominio, è possono non condivide i domini.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="f7e5e-168">Quando ogni app è ospitata nel proprio dominio, non vi è alcuna relazione di trust implicita cookie da sfruttare.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="f7e5e-169">Configurazione di ASP.NET Core non riproducibili</span><span class="sxs-lookup"><span data-stu-id="f7e5e-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="f7e5e-170">ASP.NET Core implementa usando non riproducibili [protezione dei dati di ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="f7e5e-171">Lo stack di protezione dati deve essere configurato per l'utilizzo in una server farm.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="f7e5e-172">Vedere [la configurazione di protezione dei dati](xref:security/data-protection/configuration/overview) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="f7e5e-173">In ASP.NET Core 2.0 o versione successiva, il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce il token non riproducibili in elementi del form HTML.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="f7e5e-174">Il markup seguente in un file Razor genera automaticamente i token non riproducibili:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="f7e5e-175">Analogamente, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) genera i token non riproducibili per impostazione predefinita, se il metodo del form non è GET.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="f7e5e-176">La generazione automatica di token non riproducibili per elementi del form HTML si verifica quando il `<form>` tag contiene il `method="post"` attributi e agli elementi seguenti sono vere:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="f7e5e-177">L'attributo action è vuoto (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="f7e5e-178">Non è specificato l'attributo action (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="f7e5e-179">È possibile disabilitare la generazione automatica di token non riproducibili per elementi del form HTML:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="f7e5e-180">Disabilitare in modo esplicito i token non riproducibili con il `asp-antiforgery` attributo:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="f7e5e-181">L'elemento di modulo viene scelto esplicitamente gli helper di Tag utilizzando l'Helper di Tag [! simbolo opt-out](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="f7e5e-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="f7e5e-182">Rimuovere il `FormTagHelper` dalla vista.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="f7e5e-183">Il `FormTagHelper` può essere rimosso da una vista aggiungendo la seguente direttiva alla visualizzazione Razor:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="f7e5e-184">[Pagine Razor](xref:mvc/razor-pages/index) vengono protetti automaticamente dalle XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-184">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="f7e5e-185">Per altre informazioni, vedere [XSRF/CSRF e pagine Razor](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-185">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="f7e5e-186">L'approccio più comune per difendersi da attacchi di tipo CSRF consiste nell'utilizzare il *modello di Token sincronizzazione* (STP).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="f7e5e-187">STP viene utilizzato quando l'utente richiede una pagina di dati del form:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="f7e5e-188">Il server invia un token associato all'identità dell'utente corrente al client.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="f7e5e-189">Il client invia il token per il server per la verifica.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="f7e5e-190">Se il server riceve un token che non corrisponde all'identità dell'utente autenticato, la richiesta viene rifiutata.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="f7e5e-191">Il token è univoco e imprevedibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="f7e5e-192">Il token anche utilizzabile per verificare che la sequenza corretta di una serie di richieste (ad esempio, assicurando la sequenza di richiesta di: pagina 1 &ndash; pagina 2 &ndash; pagina 3).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="f7e5e-193">Tutti i moduli nei modelli di MVC ASP.NET Core e le pagine Razor genera i token non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="f7e5e-194">La seguente coppia di esempi di vista genera i token non riproducibili:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="f7e5e-195">Aggiungere in modo esplicito un token non riproducibili a un `<form>` elemento senza utilizzare gli helper di Tag con l'helper HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="f7e5e-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="f7e5e-196">In ognuno dei casi precedenti, ASP.NET Core aggiunge un campo modulo nascosto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="f7e5e-197">ASP.NET Core include tre [filtri](xref:mvc/controllers/filters) per l'utilizzo di token non riproducibili:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="f7e5e-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="f7e5e-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="f7e5e-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="f7e5e-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="f7e5e-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="f7e5e-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="f7e5e-201">Opzioni non riproducibili</span><span class="sxs-lookup"><span data-stu-id="f7e5e-201">Antiforgery options</span></span>

<span data-ttu-id="f7e5e-202">Personalizzare [opzioni non riproducibili](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="f7e5e-203">Opzione</span><span class="sxs-lookup"><span data-stu-id="f7e5e-203">Option</span></span> | <span data-ttu-id="f7e5e-204">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f7e5e-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="f7e5e-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="f7e5e-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="f7e5e-206">Determina le impostazioni utilizzate per creare i cookie non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="f7e5e-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="f7e5e-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="f7e5e-208">Il dominio del cookie.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-208">The domain of the cookie.</span></span> <span data-ttu-id="f7e5e-209">Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-209">Defaults to `null`.</span></span> <span data-ttu-id="f7e5e-210">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="f7e5e-211">L'alternativa consigliata è Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="f7e5e-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="f7e5e-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="f7e5e-213">Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-213">The name of the cookie.</span></span> <span data-ttu-id="f7e5e-214">Se non è impostata, il sistema genera un nome univoco che inizia con il [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="f7e5e-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="f7e5e-215">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="f7e5e-216">L'alternativa consigliata è Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="f7e5e-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="f7e5e-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="f7e5e-218">Il percorso impostato nel cookie.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-218">The path set on the cookie.</span></span> <span data-ttu-id="f7e5e-219">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="f7e5e-220">L'alternativa consigliata è Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="f7e5e-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="f7e5e-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="f7e5e-222">Il nome del campo del form nascosto usato dal sistema non riproducibili per il rendering di un token non riproducibili nelle viste.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="f7e5e-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="f7e5e-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="f7e5e-224">Il nome dell'intestazione utilizzato dal sistema non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="f7e5e-225">Se `null`, il sistema prende in considerazione solo i dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="f7e5e-226">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="f7e5e-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="f7e5e-227">Specifica se SSL è richiesto dal sistema non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="f7e5e-228">Se `true`, le richieste non SSL esito negativo.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="f7e5e-229">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-229">Defaults to `false`.</span></span> <span data-ttu-id="f7e5e-230">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="f7e5e-231">L'alternativa consigliata consiste nell'impostare Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="f7e5e-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="f7e5e-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="f7e5e-233">Specifica se eliminare la generazione del `X-Frame-Options` intestazione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="f7e5e-234">Per impostazione predefinita, viene generata l'intestazione con un valore di "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="f7e5e-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="f7e5e-235">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-235">Defaults to `false`.</span></span> |

<span data-ttu-id="f7e5e-236">Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="f7e5e-237">Configurare le funzionalità non riproducibili con IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="f7e5e-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="f7e5e-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fornisce l'API per configurare le funzionalità non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="f7e5e-239">`IAntiforgery` può essere richiesta nel `Configure` metodo la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="f7e5e-240">L'esempio seguente usa middleware dalla home page dell'app per generare un token non riproducibili e inviarlo nella risposta sotto forma di cookie (tramite l'impostazione predefinita Angular convenzione di denominazione descritta più avanti in questo argomento):</span><span class="sxs-lookup"><span data-stu-id="f7e5e-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="f7e5e-241">Richiedere la convalida non riproducibili</span><span class="sxs-lookup"><span data-stu-id="f7e5e-241">Require antiforgery validation</span></span>

<span data-ttu-id="f7e5e-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) è un filtro azione che può essere applicato a una singola azione, un controller o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="f7e5e-243">Le richieste effettuate alle azioni che è sono applicato questo filtro vengono bloccate, a meno che la richiesta include un token non riproducibili valido.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="f7e5e-244">Il `ValidateAntiForgeryToken` attributo richiede un token per le richieste ai metodi di azione decora, incluse le richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="f7e5e-245">Se il `ValidateAntiForgeryToken` attributo viene applicato tra i controller dell'app, è possibile sostituirlo con il `IgnoreAntiforgeryToken` attributo.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="f7e5e-246">ASP.NET Core non supporta l'aggiunta automatica di token non riproducibili alle richieste GET.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="f7e5e-247">Convalidare automaticamente i token non riproducibili per solo i metodi HTTP non sicuri</span><span class="sxs-lookup"><span data-stu-id="f7e5e-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="f7e5e-248">Le app ASP.NET Core non generano token non riproducibili provvisoria metodi HTTP (GET, HEAD, opzioni e traccia).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="f7e5e-249">Anziché applicare su vasta scala il `ValidateAntiForgeryToken` attributo e quindi si esegue l'override con `IgnoreAntiforgeryToken` attributi, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attributo può essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="f7e5e-250">Questo attributo funziona esattamente come il `ValidateAntiForgeryToken` attributo, ad eccezione del fatto che non richiede token per le richieste effettuate utilizzando i metodi HTTP seguenti:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="f7e5e-251">GET</span><span class="sxs-lookup"><span data-stu-id="f7e5e-251">GET</span></span>
* <span data-ttu-id="f7e5e-252">HEAD</span><span class="sxs-lookup"><span data-stu-id="f7e5e-252">HEAD</span></span>
* <span data-ttu-id="f7e5e-253">OPZIONI</span><span class="sxs-lookup"><span data-stu-id="f7e5e-253">OPTIONS</span></span>
* <span data-ttu-id="f7e5e-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="f7e5e-254">TRACE</span></span>

<span data-ttu-id="f7e5e-255">Si consiglia l'uso di `AutoValidateAntiforgeryToken` su vasta scala per gli scenari non API.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="f7e5e-256">In questo modo le azioni di POST sono protetti per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="f7e5e-257">L'alternativa consiste nell'ignorare i token non riproducibili per impostazione predefinita, a meno che non `ValidateAntiForgeryToken` si riferisce a singoli metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="f7e5e-258">Non può essere più in questo scenario per un metodo di azione POST deve rimanere protetto per errore, lasciando vulnerabile agli attacchi CSRF l'app.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="f7e5e-259">Tutte le pubblicazioni devono inviare il token non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="f7e5e-260">Le API non sono un meccanismo automatico per l'invio la parte non cookie del token.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="f7e5e-261">L'implementazione dipende probabilmente l'implementazione del codice client.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="f7e5e-262">Di seguito sono illustrati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-262">Some examples are shown below:</span></span>

<span data-ttu-id="f7e5e-263">Esempio a livello di classe:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="f7e5e-264">Esempio globale:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="f7e5e-265">Eseguire l'override globale o attributi non riproducibili controller</span><span class="sxs-lookup"><span data-stu-id="f7e5e-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="f7e5e-266">Il [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro viene utilizzato per eliminare la necessità di un token non riproducibili per una determinata azione (o controller).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="f7e5e-267">Quando viene applicato, questo filtro esegue l'override `ValidateAntiForgeryToken` e `AutoValidateAntiforgeryToken` i filtri specificati a un livello superiore (a livello globale o in un controller).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="f7e5e-268">Token di aggiornamento dopo l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="f7e5e-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="f7e5e-269">I token devono essere aggiornati dopo che l'utente viene autenticato mediante il reindirizzamento all'utente a una vista o una pagina di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="f7e5e-270">JavaScript, AJAX e SPA (Single Page Application)</span><span class="sxs-lookup"><span data-stu-id="f7e5e-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="f7e5e-271">In tradizionali App basate su HTML, i token non riproducibili vengono passati al server utilizzando i campi del form nascosto.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="f7e5e-272">In App moderne basate su JavaScript e SPAs, molte richieste vengono eseguite a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="f7e5e-273">Queste richieste AJAX possono utilizzare altre tecniche (ad esempio le intestazioni di richiesta o i cookie) per inviare il token.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="f7e5e-274">Se i cookie vengono usati per archiviare i token di autenticazione e per autenticare le richieste API nel server, CSRF costituisce un potenziale problema.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="f7e5e-275">Se l'archiviazione locale viene usata per archiviare il token, CSRF vulnerabilità potrebbe essere ridotta perché i valori dall'archivio locale non vengono inviati automaticamente al server con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="f7e5e-276">Pertanto, utilizzare l'archiviazione locale per archiviare il token non riproducibili sul client che invia il token come intestazione della richiesta è un approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="f7e5e-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f7e5e-277">JavaScript</span></span>

<span data-ttu-id="f7e5e-278">Utilizzo di JavaScript con visualizzazioni, il token è possibile creare utilizzando un servizio dall'interno della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="f7e5e-279">Inserire il [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) servizio nella vista e chiamata [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="f7e5e-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="f7e5e-280">Questo approccio elimina la necessità di interagire direttamente con l'impostazione di cookie dal server o la relativa lettura dai client.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="f7e5e-281">Nell'esempio precedente Usa JavaScript per leggere il valore del campo nascosto per l'intestazione AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="f7e5e-282">JavaScript è possibile anche i token di cookie di accesso e utilizzare il contenuto del cookie per creare un'intestazione con il valore del token.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="f7e5e-283">Presupponendo che lo script richiede per inviare il token in un'intestazione denominata `X-CSRF-TOKEN`, configurare il servizio non riproducibili per cercare il `X-CSRF-TOKEN` intestazione:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="f7e5e-284">L'esempio seguente usa JavaScript per eseguire una richiesta AJAX con l'intestazione appropriata:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="f7e5e-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="f7e5e-285">AngularJS</span></span>

<span data-ttu-id="f7e5e-286">AngularJS viene utilizzata una convenzione all'indirizzo CSRF.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="f7e5e-287">Se il server invia un cookie con il nome `XSRF-TOKEN`, AngularJS `$http` servizio aggiunge il valore del cookie a un'intestazione quando invia una richiesta al server.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="f7e5e-288">Questo processo è automatico.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-288">This process is automatic.</span></span> <span data-ttu-id="f7e5e-289">L'intestazione non deve necessariamente essere impostata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="f7e5e-290">Il nome di intestazione è `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="f7e5e-291">Il server deve rilevare questa intestazione e convalidare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="f7e5e-292">Utilizzare questa convenzione per ASP.NET Core API:</span><span class="sxs-lookup"><span data-stu-id="f7e5e-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="f7e5e-293">Configurare l'app per fornire un token di un cookie denominato `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="f7e5e-294">Configurare il servizio non riproducibili per cercare un'intestazione denominata `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="f7e5e-295">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f7e5e-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="f7e5e-296">Estendere antiforgery</span><span class="sxs-lookup"><span data-stu-id="f7e5e-296">Extend antiforgery</span></span>

<span data-ttu-id="f7e5e-297">Il [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo consente agli sviluppatori di estendere il comportamento del sistema anti-CSRF, i dati aggiuntivi di round trip in ogni token.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="f7e5e-298">Il [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metodo viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="f7e5e-299">Un responsabile dell'implementazione potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore e quindi chiamare [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) per la convalida dei dati quando il token viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="f7e5e-300">Nome utente del client è già incorporato nei token generati, pertanto non è necessario includere queste informazioni.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="f7e5e-301">Se un token include dati supplementari ma nessun `IAntiForgeryAdditionalDataProvider` è configurato, i dati supplementari non convalidati.</span><span class="sxs-lookup"><span data-stu-id="f7e5e-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7e5e-302">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f7e5e-302">Additional resources</span></span>

* <span data-ttu-id="f7e5e-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sul [aprire il progetto di sicurezza dell'applicazione Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="f7e5e-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
