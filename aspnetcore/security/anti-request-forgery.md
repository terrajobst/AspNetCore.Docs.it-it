---
title: Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core
author: steve-smith
description: Informazioni su come prevenire gli attacchi contro le app web in cui un sito Web dannoso può influenzare l'interazione tra un browser client e l'app.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 88a2d127407378b9e83df7f48b1938ed081f9bb2
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208528"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="e292b-103">Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e292b-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="e292b-104">Dal [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e292b-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e292b-105">Richiesta intersito falsa (nota anche come XSRF o CSRF, pronunciato *vedere entrando*) è un attacco contro applicazioni ospitate sul web in base al quale un'app web dannoso può influenzare l'interazione tra un browser client e un'app web che considera attendibile che Browser.</span><span class="sxs-lookup"><span data-stu-id="e292b-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="e292b-106">Questi attacchi sono possibili in quanto i browser web inviano alcuni tipi di token di autenticazione automaticamente con ogni richiesta a un sito Web.</span><span class="sxs-lookup"><span data-stu-id="e292b-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="e292b-107">Questa forma di attacco è noto anche come un *attacco di un solo clic* oppure *sessione puntato* perché l'attacco sfrutta l'utente autenticato del precedentemente sessione.</span><span class="sxs-lookup"><span data-stu-id="e292b-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="e292b-108">Un esempio di un attacco CSRF:</span><span class="sxs-lookup"><span data-stu-id="e292b-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="e292b-109">Un utente accede a `www.good-banking-site.com` tramite autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="e292b-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="e292b-110">Il server autentica l'utente e invia una risposta che include un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="e292b-111">Il sito è vulnerabile agli attacchi perché considera attendibili tutte le richieste che riceve con un cookie di autenticazione valido.</span><span class="sxs-lookup"><span data-stu-id="e292b-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="e292b-112">L'utente visita un sito dannoso, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="e292b-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="e292b-113">Il sito dannoso, `www.bad-crook-site.com`, contiene un form HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e292b-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="e292b-114">Si noti che il form `action` post per il sito vulnerabile, non per il sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="e292b-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="e292b-115">Questa è la parte "intersito" di CSRF.</span><span class="sxs-lookup"><span data-stu-id="e292b-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="e292b-116">L'utente seleziona il pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="e292b-116">The user selects the submit button.</span></span> <span data-ttu-id="e292b-117">Il browser invia la richiesta e include automaticamente i cookie di autenticazione per il dominio richiesto, `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="e292b-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="e292b-118">La richiesta viene eseguita di `www.good-banking-site.com` server con il contesto di autenticazione dell'utente e può eseguire qualsiasi azione che un utente autenticato è autorizzato a eseguire.</span><span class="sxs-lookup"><span data-stu-id="e292b-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="e292b-119">Oltre allo scenario in cui l'utente seleziona il pulsante per inviare il modulo, il sito dannoso è stato possibile:</span><span class="sxs-lookup"><span data-stu-id="e292b-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="e292b-120">Eseguire uno script che invia automaticamente il form.</span><span class="sxs-lookup"><span data-stu-id="e292b-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="e292b-121">Inviare l'invio del modulo come una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="e292b-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="e292b-122">Nascondere il modulo usando lo stile CSS.</span><span class="sxs-lookup"><span data-stu-id="e292b-122">Hide the form using CSS.</span></span>

<span data-ttu-id="e292b-123">Questi scenari alternativi non richiedono qualsiasi azione e l'input dell'utente diverse da inizialmente visitando il sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="e292b-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="e292b-124">L'uso di HTTPS non impedire un attacco CSRF.</span><span class="sxs-lookup"><span data-stu-id="e292b-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="e292b-125">Il sito dannoso può inviare un `https://www.good-banking-site.com/` richiesta con la stessa semplicità può inviare una richiesta non sicura.</span><span class="sxs-lookup"><span data-stu-id="e292b-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="e292b-126">Alcuni attacchi colpiscono che rispondono alle richieste GET, nel qual caso un tag di immagine può essere utilizzato per eseguire l'azione.</span><span class="sxs-lookup"><span data-stu-id="e292b-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="e292b-127">Questa forma di attacco è comune nei siti di forum che consentono le immagini, ma Blocca JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e292b-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="e292b-128">Le app che modificano lo stato per le richieste GET, in cui le variabili o le risorse vengono modificate, sono vulnerabili ad attacchi dannosi.</span><span class="sxs-lookup"><span data-stu-id="e292b-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="e292b-129">**Le richieste GET che modificano lo stato sono non sicure. Una procedura consigliata consiste nel non modificare mai lo stato in una richiesta GET.**</span><span class="sxs-lookup"><span data-stu-id="e292b-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="e292b-130">Attacchi CSRF, sono possibili con App web che usano i cookie per l'autenticazione perché:</span><span class="sxs-lookup"><span data-stu-id="e292b-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="e292b-131">Browser archiviare i cookie emessi da un'app web.</span><span class="sxs-lookup"><span data-stu-id="e292b-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="e292b-132">I cookie stored includono i cookie di sessione per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="e292b-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="e292b-133">I browser inviano che tutti i cookie associati a un dominio all'app web di tutte le richieste indipendentemente dal modo in cui è stata generata la richiesta all'app all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="e292b-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="e292b-134">Tuttavia, gli attacchi CSRF non sono limitati a sfruttare i cookie.</span><span class="sxs-lookup"><span data-stu-id="e292b-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="e292b-135">Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="e292b-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="e292b-136">Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali finché la sessione&dagger; termina.</span><span class="sxs-lookup"><span data-stu-id="e292b-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="e292b-137">&dagger;In questo contesto *sessione* fa riferimento alla sessione dal lato client durante il quale l'utente viene autenticato.</span><span class="sxs-lookup"><span data-stu-id="e292b-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="e292b-138">È non correlata alle sessioni sul lato server oppure [Middleware di ASP.NET Core sessione](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="e292b-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="e292b-139">Gli utenti possono proteggersi contro vulnerabilità CSRF adottando delle precauzioni:</span><span class="sxs-lookup"><span data-stu-id="e292b-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="e292b-140">Firma dell'App web di termine del loro utilizzo.</span><span class="sxs-lookup"><span data-stu-id="e292b-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="e292b-141">Periodicamente i cookie del browser non crittografato.</span><span class="sxs-lookup"><span data-stu-id="e292b-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="e292b-142">Tuttavia, le vulnerabilità CSRF sono fondamentalmente un problema dell'app web, non dell'utente finale.</span><span class="sxs-lookup"><span data-stu-id="e292b-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="e292b-143">Nozioni fondamentali di autenticazione</span><span class="sxs-lookup"><span data-stu-id="e292b-143">Authentication fundamentals</span></span>

<span data-ttu-id="e292b-144">L'autenticazione basata su cookie è una forma più diffusi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="e292b-145">Sistemi di autenticazione basata su token sono sempre più diffusi, in particolare per le applicazioni a pagina singola (SPAs).</span><span class="sxs-lookup"><span data-stu-id="e292b-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="e292b-146">Autenticazione basata su cookie</span><span class="sxs-lookup"><span data-stu-id="e292b-146">Cookie-based authentication</span></span>

<span data-ttu-id="e292b-147">Quando un utente esegue l'autenticazione con nome utente e password, che siano emessi un token, contenente un ticket di autenticazione che può essere utilizzato per l'autenticazione e autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="e292b-148">Il token viene archiviato come rende un cookie associato a ogni richiesta del client.</span><span class="sxs-lookup"><span data-stu-id="e292b-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="e292b-149">Generazione e la convalida questo cookie viene eseguita dal Middleware di autenticazione dei Cookie.</span><span class="sxs-lookup"><span data-stu-id="e292b-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="e292b-150">Il [middleware](xref:fundamentals/middleware/index) serializza un'entità utente in un cookie crittografato.</span><span class="sxs-lookup"><span data-stu-id="e292b-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="e292b-151">Nelle richieste successive, il middleware convalida il cookie, ricrea l'entità e assegna l'entità per il [utente](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) proprietà di [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="e292b-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="e292b-152">Autenticazione basata su token</span><span class="sxs-lookup"><span data-stu-id="e292b-152">Token-based authentication</span></span>

<span data-ttu-id="e292b-153">Quando un utente viene autenticato, che siano emessi token (non un token anti falsificazione).</span><span class="sxs-lookup"><span data-stu-id="e292b-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="e292b-154">Il token contiene informazioni sull'utente sotto forma di [attestazioni](/dotnet/framework/security/claims-based-identity-model) o un token di riferimento che punta all'app per lo stato utente gestito nell'app.</span><span class="sxs-lookup"><span data-stu-id="e292b-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="e292b-155">Quando un utente tenta di accedere a una risorsa che richiede l'autenticazione, il token viene inviato all'app con un'intestazione di autorizzazione aggiuntive sotto forma di token di connessione.</span><span class="sxs-lookup"><span data-stu-id="e292b-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="e292b-156">In questo modo l'app senza stato.</span><span class="sxs-lookup"><span data-stu-id="e292b-156">This makes the app stateless.</span></span> <span data-ttu-id="e292b-157">In ogni richiesta successiva, il token viene passato nella richiesta per la convalida sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e292b-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="e292b-158">Questo token non è *crittografato*; ha *codificato*.</span><span class="sxs-lookup"><span data-stu-id="e292b-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="e292b-159">Nel server, il token viene decodificato per accedere alle relative informazioni.</span><span class="sxs-lookup"><span data-stu-id="e292b-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="e292b-160">Per inviare il token nelle richieste successive, archiviare il token nell'archiviazione locale del browser.</span><span class="sxs-lookup"><span data-stu-id="e292b-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="e292b-161">Non c'è da preoccuparsi delle vulnerabilità CSRF se il token viene memorizzato nell'archivio locale del browser.</span><span class="sxs-lookup"><span data-stu-id="e292b-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="e292b-162">CSRF costituisce un problema quando il token viene archiviato in un cookie.</span><span class="sxs-lookup"><span data-stu-id="e292b-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="e292b-163">Più App ospitata in un dominio</span><span class="sxs-lookup"><span data-stu-id="e292b-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="e292b-164">Ambienti di hosting condivisi sono vulnerabili a Hijack della sessione, account di accesso CSRF e altri attacchi.</span><span class="sxs-lookup"><span data-stu-id="e292b-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="e292b-165">Sebbene `example1.contoso.net` e `example2.contoso.net` sono diversi host, c'è una relazione di trust implicita tra gli host sotto il `*.contoso.net` dominio.</span><span class="sxs-lookup"><span data-stu-id="e292b-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="e292b-166">Questa relazione di trust implicita consente agli host potenzialmente non attendibili influire sul reciproci cookie (i criteri di corrispondenza dell'origine che regolano le richieste AJAX non vengono necessariamente applicano per i cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="e292b-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="e292b-167">Per impedire gli attacchi che sfruttano i cookie attendibili tra le app ospitate nello stesso dominio, è possono non condivide i domini.</span><span class="sxs-lookup"><span data-stu-id="e292b-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="e292b-168">Quando ogni app è ospitata nel proprio dominio, non vi è alcuna relazione di trust implicita cookie da sfruttare.</span><span class="sxs-lookup"><span data-stu-id="e292b-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="e292b-169">Configurazione anti falsificazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e292b-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="e292b-170">ASP.NET Core implementa tramite anti falsificazione [protezione di dati di ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="e292b-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="e292b-171">Lo stack di protezione dati deve essere configurato per funzionare in una server farm.</span><span class="sxs-lookup"><span data-stu-id="e292b-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="e292b-172">Visualizzare [configurazione della protezione dati](xref:security/data-protection/configuration/overview) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="e292b-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="e292b-173">In ASP.NET Core 2.0 o versioni successive, il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce il token anti falsificazione in elementi del form HTML.</span><span class="sxs-lookup"><span data-stu-id="e292b-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="e292b-174">Il markup seguente in un file Razor genera automaticamente i token anti falsificazione:</span><span class="sxs-lookup"><span data-stu-id="e292b-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="e292b-175">Analogamente, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) genera token antifalsificazione per impostazione predefinita, se il metodo del form non GET.</span><span class="sxs-lookup"><span data-stu-id="e292b-175">Similarly, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="e292b-176">La generazione automatica dei token antifalsificazione per elementi del form HTML si verifica quando la `<form>` tag contiene il `method="post"` attributo e uno dei modi seguenti sono vere:</span><span class="sxs-lookup"><span data-stu-id="e292b-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

* <span data-ttu-id="e292b-177">L'attributo action è vuoto (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="e292b-177">The action attribute is empty (`action=""`).</span></span>
* <span data-ttu-id="e292b-178">Non è specificato l'attributo di azione (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="e292b-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="e292b-179">È possibile disabilitare la generazione automatica dei token antifalsificazione per elementi del form HTML:</span><span class="sxs-lookup"><span data-stu-id="e292b-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="e292b-180">Disabilitare in modo esplicito i token antifalsificazione con il `asp-antiforgery` attributo:</span><span class="sxs-lookup"><span data-stu-id="e292b-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="e292b-181">L'elemento del form viene scelto esplicitamente di helper Tag usando l'Helper Tag [! esclusione simboli](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="e292b-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="e292b-182">Rimuovere il `FormTagHelper` dalla vista.</span><span class="sxs-lookup"><span data-stu-id="e292b-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="e292b-183">Il `FormTagHelper` può essere rimosso da una vista aggiungendo la seguente direttiva per la visualizzazione Razor:</span><span class="sxs-lookup"><span data-stu-id="e292b-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="e292b-184">[Razor Pages](xref:razor-pages/index) vengono protetti automaticamente dalle XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="e292b-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="e292b-185">Per altre informazioni, vedere [XSRF/CSRF e Razor Pages](xref:razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="e292b-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="e292b-186">L'approccio più comune per difendersi dagli attacchi CSRF consiste nell'usare la *modello di sincronizzazione del Token* (STP).</span><span class="sxs-lookup"><span data-stu-id="e292b-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="e292b-187">STP viene usato quando l'utente richiede una pagina con i dati del modulo:</span><span class="sxs-lookup"><span data-stu-id="e292b-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="e292b-188">Il server invia un token associato all'identità dell'utente corrente al client.</span><span class="sxs-lookup"><span data-stu-id="e292b-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="e292b-189">Il client invia il token al server per la verifica.</span><span class="sxs-lookup"><span data-stu-id="e292b-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="e292b-190">Se il server riceve un token che non corrisponde all'identità dell'utente autenticato, la richiesta viene rifiutata.</span><span class="sxs-lookup"><span data-stu-id="e292b-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="e292b-191">Il token è univoco e imprevedibili.</span><span class="sxs-lookup"><span data-stu-id="e292b-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="e292b-192">Il token è anche utilizzabile per verificare che la sequenza appropriata di una serie di richieste (ad esempio, garantendo la sequenza di richiesta di: pagina 1 &ndash; pagina 2 &ndash; pagina 3).</span><span class="sxs-lookup"><span data-stu-id="e292b-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="e292b-193">Tutti i moduli nei modelli di ASP.NET Core MVC e Razor Pages di generare token antifalsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="e292b-194">La seguente coppia di Visualizza esempi genera token anti falsificazione:</span><span class="sxs-lookup"><span data-stu-id="e292b-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="e292b-195">Aggiungere in modo esplicito un token anti falsificazione a un `<form>` elemento senza usare gli helper Tag con l'helper HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="e292b-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="e292b-196">In ognuno dei casi precedenti, ASP.NET Core aggiunge un campo del form nascosto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e292b-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="e292b-197">ASP.NET Core include tre [filtri](xref:mvc/controllers/filters) per l'utilizzo di token anti falsificazione:</span><span class="sxs-lookup"><span data-stu-id="e292b-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="e292b-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="e292b-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="e292b-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="e292b-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="e292b-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="e292b-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="e292b-201">Opzioni di anti falsificazione</span><span class="sxs-lookup"><span data-stu-id="e292b-201">Antiforgery options</span></span>

<span data-ttu-id="e292b-202">Personalizzare [opzioni di anti falsificazione](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e292b-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="e292b-203">&dagger;Impostare l'anti falsificazione `Cookie` delle proprietà usando la proprietà dell'oggetto di [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) classe.</span><span class="sxs-lookup"><span data-stu-id="e292b-203">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="e292b-204">Opzione</span><span class="sxs-lookup"><span data-stu-id="e292b-204">Option</span></span> | <span data-ttu-id="e292b-205">Descrizione</span><span class="sxs-lookup"><span data-stu-id="e292b-205">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e292b-206">Cookie</span><span class="sxs-lookup"><span data-stu-id="e292b-206">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="e292b-207">Determina le impostazioni utilizzate per creare i cookie antifalsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-207">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="e292b-208">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="e292b-208">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="e292b-209">Il nome del campo del form nascosto utilizzato dal sistema antifalsificazione per il rendering di un token anti falsificazione in viste.</span><span class="sxs-lookup"><span data-stu-id="e292b-209">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="e292b-210">HeaderName</span><span class="sxs-lookup"><span data-stu-id="e292b-210">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="e292b-211">Il nome dell'intestazione utilizzato dal sistema antifalsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-211">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="e292b-212">Se `null`, il sistema prende in considerazione solo i dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="e292b-212">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="e292b-213">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="e292b-213">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="e292b-214">Specifica se eliminare la generazione del `X-Frame-Options` intestazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-214">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="e292b-215">Per impostazione predefinita, l'intestazione viene generato con un valore di "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="e292b-215">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="e292b-216">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="e292b-216">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

| <span data-ttu-id="e292b-217">Opzione</span><span class="sxs-lookup"><span data-stu-id="e292b-217">Option</span></span> | <span data-ttu-id="e292b-218">Descrizione</span><span class="sxs-lookup"><span data-stu-id="e292b-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e292b-219">Cookie</span><span class="sxs-lookup"><span data-stu-id="e292b-219">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="e292b-220">Determina le impostazioni utilizzate per creare i cookie antifalsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-220">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="e292b-221">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="e292b-221">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="e292b-222">Il dominio del cookie.</span><span class="sxs-lookup"><span data-stu-id="e292b-222">The domain of the cookie.</span></span> <span data-ttu-id="e292b-223">Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="e292b-223">Defaults to `null`.</span></span> <span data-ttu-id="e292b-224">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="e292b-224">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e292b-225">L'alternativa consigliata è Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="e292b-225">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="e292b-226">CookieName</span><span class="sxs-lookup"><span data-stu-id="e292b-226">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="e292b-227">Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="e292b-227">The name of the cookie.</span></span> <span data-ttu-id="e292b-228">Se non impostato, il sistema genera un nome univoco che inizia con la [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="e292b-228">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="e292b-229">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="e292b-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e292b-230">L'alternativa consigliata è Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="e292b-230">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="e292b-231">CookiePath</span><span class="sxs-lookup"><span data-stu-id="e292b-231">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="e292b-232">Il percorso impostato nel cookie.</span><span class="sxs-lookup"><span data-stu-id="e292b-232">The path set on the cookie.</span></span> <span data-ttu-id="e292b-233">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="e292b-233">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e292b-234">L'alternativa consigliata è Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="e292b-234">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="e292b-235">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="e292b-235">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="e292b-236">Il nome del campo del form nascosto utilizzato dal sistema antifalsificazione per il rendering di un token anti falsificazione in viste.</span><span class="sxs-lookup"><span data-stu-id="e292b-236">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="e292b-237">HeaderName</span><span class="sxs-lookup"><span data-stu-id="e292b-237">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="e292b-238">Il nome dell'intestazione utilizzato dal sistema antifalsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-238">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="e292b-239">Se `null`, il sistema prende in considerazione solo i dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="e292b-239">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="e292b-240">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="e292b-240">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="e292b-241">Specifica se il sistema antifalsificazione richiede HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e292b-241">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="e292b-242">Se `true`, non HTTPS richieste hanno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="e292b-242">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="e292b-243">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="e292b-243">Defaults to `false`.</span></span> <span data-ttu-id="e292b-244">Questa proprietà è obsoleta e verrà rimossa in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="e292b-244">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e292b-245">L'alternativa consigliata consiste nell'impostare Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="e292b-245">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="e292b-246">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="e292b-246">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="e292b-247">Specifica se eliminare la generazione del `X-Frame-Options` intestazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-247">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="e292b-248">Per impostazione predefinita, l'intestazione viene generato con un valore di "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="e292b-248">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="e292b-249">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="e292b-249">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="e292b-250">Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="e292b-250">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="e292b-251">Configurare le funzionalità anti falsificazione con IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="e292b-251">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="e292b-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fornisce l'API per configurare le funzionalità di anti falsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="e292b-253">`IAntiforgery` può essere richiesta nel `Configure` metodo di `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="e292b-253">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="e292b-254">L'esempio seguente usa middleware dalla home page dell'app per generare un token anti falsificazione e inviarlo nella risposta sotto forma di cookie (tramite l'impostazione predefinita Angular convenzione di denominazione descritta più avanti in questo argomento):</span><span class="sxs-lookup"><span data-stu-id="e292b-254">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="e292b-255">Richiedere la convalida antifalsificazione</span><span class="sxs-lookup"><span data-stu-id="e292b-255">Require antiforgery validation</span></span>

<span data-ttu-id="e292b-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) è un filtro azione che può essere applicato a una singola azione, un controller o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="e292b-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="e292b-257">Le richieste effettuate alle azioni che hanno applicato questo filtro vengono bloccate a meno che la richiesta include un token anti falsificazione valido.</span><span class="sxs-lookup"><span data-stu-id="e292b-257">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="e292b-258">Il `ValidateAntiForgeryToken` attributo richiede un token per le richieste ai metodi di azione che decora, tra cui le richieste HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e292b-258">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="e292b-259">Se il `ValidateAntiForgeryToken` attributo è applicato tra controller dell'app, che può essere sostituito con il `IgnoreAntiforgeryToken` attributo.</span><span class="sxs-lookup"><span data-stu-id="e292b-259">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="e292b-260">ASP.NET Core non supporta l'aggiunta di token anti falsificazione alle richieste GET automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e292b-260">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="e292b-261">Convalidare automaticamente i token antifalsificazione per solo i metodi HTTP non sicuri</span><span class="sxs-lookup"><span data-stu-id="e292b-261">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="e292b-262">Le app ASP.NET Core non generano i token antifalsificazione per safe metodi HTTP (GET, HEAD, opzioni e traccia).</span><span class="sxs-lookup"><span data-stu-id="e292b-262">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="e292b-263">Anziché applicare ampiamente la `ValidateAntiForgeryToken` attributo e quindi si esegue l'override con `IgnoreAntiforgeryToken` attributi, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attributo può essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="e292b-263">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="e292b-264">Questo attributo funziona in modo identico al `ValidateAntiForgeryToken` attributo, ad eccezione del fatto che non richiede token per le richieste effettuate usando i metodi HTTP seguenti:</span><span class="sxs-lookup"><span data-stu-id="e292b-264">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="e292b-265">GET</span><span class="sxs-lookup"><span data-stu-id="e292b-265">GET</span></span>
* <span data-ttu-id="e292b-266">HEAD</span><span class="sxs-lookup"><span data-stu-id="e292b-266">HEAD</span></span>
* <span data-ttu-id="e292b-267">OPZIONI</span><span class="sxs-lookup"><span data-stu-id="e292b-267">OPTIONS</span></span>
* <span data-ttu-id="e292b-268">TRACE</span><span class="sxs-lookup"><span data-stu-id="e292b-268">TRACE</span></span>

<span data-ttu-id="e292b-269">È consigliabile usare `AutoValidateAntiforgeryToken` su vasta scala per gli scenari non-API.</span><span class="sxs-lookup"><span data-stu-id="e292b-269">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="e292b-270">In questo modo le azioni POST sono protetti per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e292b-270">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="e292b-271">In alternativa è possibile ignorare i token antifalsificazione per impostazione predefinita, a meno che non `ValidateAntiForgeryToken` viene applicata a singoli metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="e292b-271">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="e292b-272">È più probabile che in questo scenario per un metodo di azione POST deve rimanere non protetto per errore, uscire dall'app vulnerabile agli attacchi CSRF.</span><span class="sxs-lookup"><span data-stu-id="e292b-272">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="e292b-273">Tutti i post devono inviare il token antifalsificazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-273">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="e292b-274">Le API non sono un meccanismo automatico per inviare la parte non cookie del token.</span><span class="sxs-lookup"><span data-stu-id="e292b-274">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="e292b-275">L'implementazione dipende probabilmente dall'implementazione del codice client.</span><span class="sxs-lookup"><span data-stu-id="e292b-275">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="e292b-276">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="e292b-276">Some examples are shown below:</span></span>

<span data-ttu-id="e292b-277">Esempio a livello di classe:</span><span class="sxs-lookup"><span data-stu-id="e292b-277">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="e292b-278">Esempio globale:</span><span class="sxs-lookup"><span data-stu-id="e292b-278">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="e292b-279">Override globale o controller anti falsificazione attributi</span><span class="sxs-lookup"><span data-stu-id="e292b-279">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="e292b-280">Il [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro viene utilizzato per eliminare la necessità di un token anti falsificazione per una determinata azione (o controller).</span><span class="sxs-lookup"><span data-stu-id="e292b-280">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="e292b-281">Quando applicata, esegue l'override di questo filtro `ValidateAntiForgeryToken` e `AutoValidateAntiforgeryToken` filtri specificati a un livello superiore (a livello globale o in un controller).</span><span class="sxs-lookup"><span data-stu-id="e292b-281">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="e292b-282">Token di aggiornamento dopo l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="e292b-282">Refresh tokens after authentication</span></span>

<span data-ttu-id="e292b-283">I token devono essere aggiornati dopo che l'utente viene autenticato, reindirizzare l'utente a una pagina Razor Pages o una vista.</span><span class="sxs-lookup"><span data-stu-id="e292b-283">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="e292b-284">JavaScript, AJAX e SPA (Single Page Application)</span><span class="sxs-lookup"><span data-stu-id="e292b-284">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="e292b-285">Nelle App basate su HTML tradizionale, i token anti falsificazione vengono passati al server usando i campi modulo nascosti.</span><span class="sxs-lookup"><span data-stu-id="e292b-285">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="e292b-286">Nell'App moderne basate su JavaScript e SPAs, molte richieste vengono effettuate a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="e292b-286">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="e292b-287">Queste richieste AJAX possono utilizzare altre tecniche, quali intestazioni della richiesta o i cookie, inviare il token.</span><span class="sxs-lookup"><span data-stu-id="e292b-287">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="e292b-288">Se i cookie vengono usati per archiviare i token di autenticazione e per autenticare le richieste di API sul server, CSRF è un potenziale problema.</span><span class="sxs-lookup"><span data-stu-id="e292b-288">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="e292b-289">Se l'archiviazione locale viene usata per archiviare il token, CSRF vulnerabilità potrebbe essere ridotta perché i valori dall'archiviazione locale non vengono inviati automaticamente al server con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="e292b-289">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="e292b-290">Di conseguenza, usando un'archiviazione locale per archiviare il token anti falsificazione sul client che invia il token come intestazione della richiesta è un approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="e292b-290">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="e292b-291">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e292b-291">JavaScript</span></span>

<span data-ttu-id="e292b-292">Utilizzo di JavaScript con visualizzazioni, il token è possibile creare usando un servizio dall'interno della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e292b-292">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="e292b-293">Inserire il [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) servizio nella visualizzazione e chiamare [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="e292b-293">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="e292b-294">Questo approccio elimina la necessità di interagire direttamente con l'impostazione di cookie dal server o la relativa lettura dai client.</span><span class="sxs-lookup"><span data-stu-id="e292b-294">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="e292b-295">L'esempio precedente Usa JavaScript per leggere il valore del campo nascosto per l'intestazione di AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="e292b-295">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="e292b-296">JavaScript è possibile anche i token nei cookie di accesso e usare il contenuto del cookie per creare un'intestazione con il valore del token.</span><span class="sxs-lookup"><span data-stu-id="e292b-296">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="e292b-297">Supponendo che lo script richiede di inviare il token in un'intestazione denominata `X-CSRF-TOKEN`, configurare il servizio anti falsificazione per cercare il `X-CSRF-TOKEN` intestazione:</span><span class="sxs-lookup"><span data-stu-id="e292b-297">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="e292b-298">L'esempio seguente usa JavaScript per eseguire una richiesta AJAX con l'intestazione appropriata:</span><span class="sxs-lookup"><span data-stu-id="e292b-298">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="e292b-299">AngularJS</span><span class="sxs-lookup"><span data-stu-id="e292b-299">AngularJS</span></span>

<span data-ttu-id="e292b-300">AngularJS Usa una convenzione all'indirizzo CSRF.</span><span class="sxs-lookup"><span data-stu-id="e292b-300">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="e292b-301">Se il server invia un cookie con il nome `XSRF-TOKEN`, AngularJS `$http` servizio aggiunge il valore del cookie a un'intestazione quando viene inviata una richiesta al server.</span><span class="sxs-lookup"><span data-stu-id="e292b-301">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="e292b-302">Questo processo è automatico.</span><span class="sxs-lookup"><span data-stu-id="e292b-302">This process is automatic.</span></span> <span data-ttu-id="e292b-303">L'intestazione non deve essere impostata in modo esplicito nel client.</span><span class="sxs-lookup"><span data-stu-id="e292b-303">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="e292b-304">È il nome dell'intestazione `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="e292b-304">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="e292b-305">Il server deve rilevare questa intestazione e convalidare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="e292b-305">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="e292b-306">Per ASP.NET Core API da usare con questa convenzione nell'avvio dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="e292b-306">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="e292b-307">Configurare l'app per fornire un token in un cookie denominato `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="e292b-307">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="e292b-308">Configurare il servizio per individuare un'intestazione denominata anti falsificazione `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="e292b-308">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

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

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="e292b-309">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e292b-309">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="e292b-310">Estendere anti falsificazione</span><span class="sxs-lookup"><span data-stu-id="e292b-310">Extend antiforgery</span></span>

<span data-ttu-id="e292b-311">Il [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo consente agli sviluppatori di estendere il comportamento del sistema Intersito, i dati aggiuntivi di andata e ritorno in ogni token.</span><span class="sxs-lookup"><span data-stu-id="e292b-311">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="e292b-312">Il [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato.</span><span class="sxs-lookup"><span data-stu-id="e292b-312">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="e292b-313">Un implementatore potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore, quindi chiamare [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) per convalidare i dati quando il token viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="e292b-313">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="e292b-314">Nome utente del client è già incorporato nei token generati, pertanto non è necessario includere queste informazioni.</span><span class="sxs-lookup"><span data-stu-id="e292b-314">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="e292b-315">Se un token include dati aggiuntivi, ma non `IAntiForgeryAdditionalDataProvider` è configurato, non vero convalidati dati aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="e292b-315">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e292b-316">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e292b-316">Additional resources</span></span>

* <span data-ttu-id="e292b-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sul [aprire Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="e292b-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
