---
title: Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core
author: steve-smith
description: Informazioni su come prevenire gli attacchi contro le app web in un sito Web dannoso può influenzare l'interazione tra un browser del client e l'app.
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="231e6-103">Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="231e6-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="231e6-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="231e6-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="231e6-105">Quali attacchi previene l'antifalsificazione?</span><span class="sxs-lookup"><span data-stu-id="231e6-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="231e6-106">Richieste intersito false (noto anche come XSRF o CSRF, pronuncia *tra superfici vedere*) è un attacco contro applicazioni ospitate da web in base al quale un sito web in grado di influenzare l'interazione tra un browser del client e un sito web che considera attendibile tale browser.</span><span class="sxs-lookup"><span data-stu-id="231e6-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="231e6-107">Questi attacchi sono possibili in quanto web browser invia alcuni tipi di token di autenticazione automaticamente con ogni richiesta a un sito web.</span><span class="sxs-lookup"><span data-stu-id="231e6-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="231e6-108">Questa forma di attacco del noto anche come un *attacco con un clic* o come *sessione riding*, perché l'attacco sfrutta i vantaggi dell'utente dell'autenticazione in precedenza sessione.</span><span class="sxs-lookup"><span data-stu-id="231e6-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="231e6-109">Un esempio di un attacco di tipo CSRF:</span><span class="sxs-lookup"><span data-stu-id="231e6-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="231e6-110">Un utente accede a `www.example.com`, con autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="231e6-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="231e6-111">Il server autentica l'utente e invia una risposta che include un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="231e6-112">L'utente visita un sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="231e6-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="231e6-113">Il sito dannoso contiene un form HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="231e6-113">The malicious site contains an HTML form similar to the following:</span></span>

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

<span data-ttu-id="231e6-114">Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="231e6-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="231e6-115">Questa è la parte "cross-site" di CSRF.</span><span class="sxs-lookup"><span data-stu-id="231e6-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="231e6-116">L'utente fa clic sul pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="231e6-116">The user clicks the submit button.</span></span> <span data-ttu-id="231e6-117">Il browser include automaticamente il cookie di autenticazione per il dominio richiesto (sito vulnerabile in questo caso) con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="231e6-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="231e6-118">La richiesta viene eseguita nel server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che è consentita ad un utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="231e6-118">The request runs on the server with the user's authentication context and can perform any action that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="231e6-119">Questo esempio richiede all'utente di fare clic sul pulsante del form.</span><span class="sxs-lookup"><span data-stu-id="231e6-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="231e6-120">La pagina dannosa potrebbe:</span><span class="sxs-lookup"><span data-stu-id="231e6-120">The malicious page could:</span></span>

* <span data-ttu-id="231e6-121">Eseguire uno script che invia automaticamente il form.</span><span class="sxs-lookup"><span data-stu-id="231e6-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="231e6-122">Inviare il form come una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="231e6-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="231e6-123">Utilizzare un form nascosto con CSS.</span><span class="sxs-lookup"><span data-stu-id="231e6-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="231e6-124">Un attacco di tipo CSRF non impedisce l'uso di SSL, il sito dannoso può inviare una richiesta `https://`.</span><span class="sxs-lookup"><span data-stu-id="231e6-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="231e6-125">Alcuni attacchi hanno come oggetto gli endpoint del sito che rispondono alle richieste`GET` , nel qual caso un tag image consente di eseguire l'azione (questa forma di attacco è comune nei siti di forum che consentono le immagini ma bloccano JavaScript).</span><span class="sxs-lookup"><span data-stu-id="231e6-125">Some attacks target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="231e6-126">Le applicazioni che modificano lo stato con richieste `GET` sono vulnerabili a questi attacchi.</span><span class="sxs-lookup"><span data-stu-id="231e6-126">Applications that change state with `GET` requests are vulnerable from malicious attacks.</span></span>

<span data-ttu-id="231e6-127">Attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, in quanto browser invia tutti i cookie pertinenti al sito web di destinazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="231e6-128">Tuttavia, gli attacchi CSRF non sono limitati per sfruttare i cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="231e6-129">Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="231e6-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="231e6-130">Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali, fino al termine della sessione.</span><span class="sxs-lookup"><span data-stu-id="231e6-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="231e6-131">Nota: In questo contesto, *sessione* fa riferimento alla sessione sul lato client durante la quale l'utente viene autenticato.</span><span class="sxs-lookup"><span data-stu-id="231e6-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="231e6-132">Non è correlato alle sessioni lato server o alla [sessione middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="231e6-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="231e6-133">Gli utenti possono impedire vulnerabilità CSRF in questo modo:</span><span class="sxs-lookup"><span data-stu-id="231e6-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="231e6-134">Disconnettendosi da siti web quando hanno finito di usarli.</span><span class="sxs-lookup"><span data-stu-id="231e6-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="231e6-135">Cancellando periodicamente i cookie del browser.</span><span class="sxs-lookup"><span data-stu-id="231e6-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="231e6-136">Tuttavia, le vulnerabilità CSRF sono fondamentalmente un problema dell'app web, non dell'utente finale.</span><span class="sxs-lookup"><span data-stu-id="231e6-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="231e6-137">Modalità di ASP.NET MVC di base indirizzi CSRF?</span><span class="sxs-lookup"><span data-stu-id="231e6-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="231e6-138">ASP.NET Core implementa request anti-falsificazione usando il [dello stack di protezione dati di ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="231e6-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="231e6-139">La protezione dei dati di ASP.NET Core deve essere configurato per funzionare in una server farm.</span><span class="sxs-lookup"><span data-stu-id="231e6-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="231e6-140">Vedere [la configurazione di protezione dei dati](xref:security/data-protection/configuration/overview) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="231e6-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="231e6-141">Configurazione di protezione predefinito request anti-falsificazione ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="231e6-141">ASP.NET Core anti-request-forgery default data protection configuration</span></span> 

<span data-ttu-id="231e6-142">In ASP.NET Core MVC 2.0 il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce i token antifalsificazione per elementi del form HTML.</span><span class="sxs-lookup"><span data-stu-id="231e6-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="231e6-143">Ad esempio, il markup seguente in un file Razor genererà automaticamente i token antifalsificazione:</span><span class="sxs-lookup"><span data-stu-id="231e6-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="231e6-144">La generazione automatica di token antifalsificazione per elementi del form HTML si verifica quando:</span><span class="sxs-lookup"><span data-stu-id="231e6-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="231e6-145">Il `form` tag contiene il `method="post"` attributo AND</span><span class="sxs-lookup"><span data-stu-id="231e6-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="231e6-146">L'attributo action è vuoto.</span><span class="sxs-lookup"><span data-stu-id="231e6-146">The action attribute is empty.</span></span> <span data-ttu-id="231e6-147">( `action=""`) O</span><span class="sxs-lookup"><span data-stu-id="231e6-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="231e6-148">Non è specificato l'attributo dell'azione.</span><span class="sxs-lookup"><span data-stu-id="231e6-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="231e6-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="231e6-149">(`<form method="post">`)</span></span>

<span data-ttu-id="231e6-150">È possibile disabilitare la generazione automatica di token antifalsificazione per elementi del form HTML da:</span><span class="sxs-lookup"><span data-stu-id="231e6-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="231e6-151">Disabilita in modo esplicito `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="231e6-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="231e6-152">Esempio:</span><span class="sxs-lookup"><span data-stu-id="231e6-152">For example</span></span>

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="231e6-153">Scegliere l'elemento form fuori gli helper di Tag utilizzando l'Helper di Tag [! simbolo rinuncia](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="231e6-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

  ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="231e6-154">Rimuovere il `FormTagHelper` dalla vista.</span><span class="sxs-lookup"><span data-stu-id="231e6-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="231e6-155">È possibile rimuovere il `FormTagHelper` da una vista aggiungendo la seguente direttiva alla visualizzazione Razor:</span><span class="sxs-lookup"><span data-stu-id="231e6-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="231e6-156">[Pagine Razor](xref:mvc/razor-pages/index) vengono protetti automaticamente dalle XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="231e6-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="231e6-157">Non è necessario scrivere codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="231e6-157">You don't have to write any additional code.</span></span> <span data-ttu-id="231e6-158">Vedere [XSRF/CSRF e pagine Razor](xref:mvc/razor-pages/index#xsrf) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="231e6-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="231e6-159">L'approccio più comune di difesa contro gli attacchi CSRF è il modello di token del programma di sincronizzazione (STP).</span><span class="sxs-lookup"><span data-stu-id="231e6-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="231e6-160">STP è una tecnica usata quando l'utente richiede una pagina di dati del form.</span><span class="sxs-lookup"><span data-stu-id="231e6-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="231e6-161">Il server invia un token associato all'identità dell'utente corrente al client.</span><span class="sxs-lookup"><span data-stu-id="231e6-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="231e6-162">Il client invia il token per il server per la verifica.</span><span class="sxs-lookup"><span data-stu-id="231e6-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="231e6-163">Se il server riceve un token che non corrisponde all'identità dell'utente autenticato, la richiesta viene rifiutata.</span><span class="sxs-lookup"><span data-stu-id="231e6-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="231e6-164">Il token è univoco e imprevedibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="231e6-165">Il token può essere utilizzato anche per garantire la corretta sequenza di una serie di richieste (pagina verifica 1 precede pagina 2 che precede pagina 3).</span><span class="sxs-lookup"><span data-stu-id="231e6-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="231e6-166">Tutti i moduli nei modelli ASP.NET MVC Core generano token non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="231e6-167">I due esempi seguenti di logica di visualizzazione generano token non riproducibili:</span><span class="sxs-lookup"><span data-stu-id="231e6-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="231e6-168">È possibile aggiungere in modo esplicito un token non riproducibili a un `<form>` elemento senza utilizzare gli helper di tag con l'helper HTML `@Html.AntiForgeryToken`:</span><span class="sxs-lookup"><span data-stu-id="231e6-168">You can explicitly add an antiforgery token to a `<form>` element without using tag helpers with the HTML helper `@Html.AntiForgeryToken`:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="231e6-169">In ognuno dei casi precedenti, ASP.NET Core verranno aggiunti a un campo modulo nascosto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="231e6-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

<span data-ttu-id="231e6-170">ASP.NET Core include tre [filtri](xref:mvc/controllers/filters) per l'utilizzo di token non riproducibili: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, e `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="231e6-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, and `IgnoreAntiforgeryToken`.</span></span>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="231e6-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="231e6-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="231e6-172">Il `ValidateAntiForgeryToken` è un filtro azione che può essere applicato a una singola azione, un controller, o a livello globale.</span><span class="sxs-lookup"><span data-stu-id="231e6-172">The `ValidateAntiForgeryToken` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="231e6-173">Le richieste effettuate alle azioni che è sono applicato il filtro verranno bloccate a meno che la richiesta include un token valido non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="231e6-174">Il `ValidateAntiForgeryToken` attributo richiede un token per le richieste a metodi di azione decora, tra cui `HTTP GET` richieste.</span><span class="sxs-lookup"><span data-stu-id="231e6-174">The `ValidateAntiForgeryToken` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="231e6-175">Se si applica su vasta scala, è possibile eseguirne l'override con il `IgnoreAntiforgeryToken` attributo.</span><span class="sxs-lookup"><span data-stu-id="231e6-175">If you apply it broadly, you can override it with the `IgnoreAntiforgeryToken` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="231e6-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="231e6-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="231e6-177">Le applicazioni ASP.NET Core in genere non generano token non riproducibili per i metodi HTTP sicuro (GET, HEAD, opzioni e traccia).</span><span class="sxs-lookup"><span data-stu-id="231e6-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="231e6-178">Anziché applicare ampiamente la `ValidateAntiForgeryToken` attributo e quindi si esegue l'override con `IgnoreAntiforgeryToken` gli attributi, è possibile utilizzare il ``AutoValidateAntiforgeryToken`` attributo.</span><span class="sxs-lookup"><span data-stu-id="231e6-178">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="231e6-179">Questo attributo funziona esattamente come il `ValidateAntiForgeryToken` attributo, ad eccezione del fatto che non richiede token per le richieste effettuate utilizzando i metodi HTTP seguenti:</span><span class="sxs-lookup"><span data-stu-id="231e6-179">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="231e6-180">GET</span><span class="sxs-lookup"><span data-stu-id="231e6-180">GET</span></span>
* <span data-ttu-id="231e6-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="231e6-181">HEAD</span></span>
* <span data-ttu-id="231e6-182">OPZIONI</span><span class="sxs-lookup"><span data-stu-id="231e6-182">OPTIONS</span></span>
* <span data-ttu-id="231e6-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="231e6-183">TRACE</span></span>

<span data-ttu-id="231e6-184">Si consiglia di usare `AutoValidateAntiforgeryToken` su vasta scala per gli scenari non API.</span><span class="sxs-lookup"><span data-stu-id="231e6-184">We recommend you use `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="231e6-185">In questo modo che le azioni di POST sono protetti per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="231e6-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="231e6-186">L'alternativa consiste nell'ignorare i token non riproducibili per impostazione predefinita, a meno che non `ValidateAntiForgeryToken` viene applicato al metodo di azione singola.</span><span class="sxs-lookup"><span data-stu-id="231e6-186">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to the individual action method.</span></span> <span data-ttu-id="231e6-187">È più probabile che in questo scenario per un metodo di azione di POST di sinistra non è protetta, lasciando vulnerabile agli attacchi CSRF l'app.</span><span class="sxs-lookup"><span data-stu-id="231e6-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="231e6-188">POST anche anonimo deve inviare il token non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="231e6-189">Nota: Le API non disporre di un meccanismo automatico per l'invio la parte non cookie del token. l'implementazione dipenderà probabilmente l'implementazione del codice client.</span><span class="sxs-lookup"><span data-stu-id="231e6-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="231e6-190">Di seguito sono illustrati alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="231e6-190">Some examples are shown below.</span></span>

<span data-ttu-id="231e6-191">Esempio (livello di classe):</span><span class="sxs-lookup"><span data-stu-id="231e6-191">Example (class level):</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="231e6-192">Esempio (globale):</span><span class="sxs-lookup"><span data-stu-id="231e6-192">Example (global):</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="231e6-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="231e6-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="231e6-194">Il `IgnoreAntiforgeryToken` filtro viene utilizzato per eliminare la necessità di un token di essere presenti per una determinata azione o controller, non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-194">The `IgnoreAntiforgeryToken` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="231e6-195">Quando viene applicato, sostituirà questo filtro `ValidateAntiForgeryToken` e/o `AutoValidateAntiforgeryToken` i filtri specificati a un livello superiore (a livello globale o in un controller).</span><span class="sxs-lookup"><span data-stu-id="231e6-195">When applied, this filter will override `ValidateAntiForgeryToken` and/or `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="231e6-196">JavaScript, AJAX e SPA (Single Page Application)</span><span class="sxs-lookup"><span data-stu-id="231e6-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="231e6-197">Nelle applicazioni basate su HTML tradizionale, i token non riproducibili vengono passati al server utilizzando i campi del form nascosto.</span><span class="sxs-lookup"><span data-stu-id="231e6-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="231e6-198">In App moderne basate su JavaScript e applicazioni a pagina singola (SPAs), numero di richieste viene eseguite a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="231e6-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="231e6-199">Queste richieste AJAX possono utilizzare altre tecniche (ad esempio le intestazioni di richiesta o i cookie) per inviare il token.</span><span class="sxs-lookup"><span data-stu-id="231e6-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="231e6-200">Se i cookie vengono usati per archiviare i token di autenticazione e per autenticare le richieste API nel server, CSRF sarà un potenziale problema.</span><span class="sxs-lookup"><span data-stu-id="231e6-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="231e6-201">Tuttavia, se l'archiviazione locale viene utilizzato per memorizzare il token, CSRF vulnerabilità può essere contrastata, poiché i valori dall'archivio locale non vengono inviati automaticamente al server con ogni nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="231e6-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="231e6-202">Pertanto, utilizzare l'archiviazione locale per archiviare il token non riproducibili sul client che invia il token come intestazione della richiesta è un approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="231e6-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="231e6-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="231e6-203">AngularJS</span></span>

<span data-ttu-id="231e6-204">AngularJS viene utilizzata una convenzione all'indirizzo CSRF.</span><span class="sxs-lookup"><span data-stu-id="231e6-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="231e6-205">Se il server invia un cookie con il nome `XSRF-TOKEN`, l'angolare `$http` servizio aggiungerà il valore da questo cookie a un'intestazione quando invia una richiesta al server.</span><span class="sxs-lookup"><span data-stu-id="231e6-205">If the server sends a cookie with the name `XSRF-TOKEN`, the Angular `$http` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="231e6-206">Questo processo è automatico; non è necessario impostare in modo esplicito l'intestazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="231e6-207">Il nome di intestazione è `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="231e6-207">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="231e6-208">Il server deve rilevare questa intestazione e convalidare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="231e6-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="231e6-209">Utilizzare questa convenzione per ASP.NET Core API:</span><span class="sxs-lookup"><span data-stu-id="231e6-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="231e6-210">Configurare l'app per fornire un token di un cookie denominato `XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="231e6-210">Configure your app to provide a token in a cookie called `XSRF-TOKEN`</span></span>
* <span data-ttu-id="231e6-211">Configurare il servizio non riproducibili per cercare un'intestazione denominata `X-XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="231e6-211">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="231e6-212">[Visualizzare l'esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="231e6-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="231e6-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="231e6-213">JavaScript</span></span>

<span data-ttu-id="231e6-214">Utilizzo di JavaScript con visualizzazioni, è possibile creare il token utilizzando un servizio dall'interno della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="231e6-215">A tale scopo, inserisce il `Microsoft.AspNetCore.Antiforgery.IAntiforgery` servizio nella vista e chiamata `GetAndStoreTokens`, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="231e6-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

<span data-ttu-id="231e6-216">Questo approccio elimina la necessità di interagire direttamente con l'impostazione di cookie dal server o la relativa lettura dai client.</span><span class="sxs-lookup"><span data-stu-id="231e6-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="231e6-217">Nell'esempio precedente utilizza jQuery per leggere il valore del campo nascosto per l'intestazione AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="231e6-217">The preceding example uses jQuery to read the hidden field value for the AJAX POST header.</span></span> <span data-ttu-id="231e6-218">Per utilizzare JavaScript per ottenere il valore del token, utilizzare `document.getElementById('RequestVerificationToken').value`.</span><span class="sxs-lookup"><span data-stu-id="231e6-218">To use JavaScript to obtain the token's value, use `document.getElementById('RequestVerificationToken').value`.</span></span>

<span data-ttu-id="231e6-219">JavaScript è possibile anche i token forniti nei cookie di accesso e quindi utilizzare il contenuto del cookie per creare un'intestazione con il valore del token, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="231e6-219">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="231e6-220">Quindi, presupponendo che si crea lo script richiede per inviare il token in un'intestazione denominata `X-CSRF-TOKEN`, configurare il servizio non riproducibili per cercare il `X-CSRF-TOKEN` intestazione:</span><span class="sxs-lookup"><span data-stu-id="231e6-220">Then, assuming you construct your script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="231e6-221">L'esempio seguente usa jQuery per effettuare una richiesta AJAX con l'intestazione appropriata:</span><span class="sxs-lookup"><span data-stu-id="231e6-221">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="231e6-222">Configurazione Antiforgery</span><span class="sxs-lookup"><span data-stu-id="231e6-222">Configuring Antiforgery</span></span>

<span data-ttu-id="231e6-223">`IAntiforgery` fornisce l'API per configurare il sistema non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-223">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="231e6-224">Può essere richiesta nel `Configure` metodo la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="231e6-224">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="231e6-225">L'esempio seguente usa middleware dalla home page dell'app per generare un token non riproducibili e inviarlo nella risposta sotto forma di cookie (impostazione predefinita angolare convenzioni di denominazione descritte in precedenza):</span><span class="sxs-lookup"><span data-stu-id="231e6-225">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```csharp
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="231e6-226">Opzioni</span><span class="sxs-lookup"><span data-stu-id="231e6-226">Options</span></span>

<span data-ttu-id="231e6-227">È possibile personalizzare [opzioni non riproducibili](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="231e6-227">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "mydomain.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="231e6-228">Opzione</span><span class="sxs-lookup"><span data-stu-id="231e6-228">Option</span></span>        | <span data-ttu-id="231e6-229">Descrizione</span><span class="sxs-lookup"><span data-stu-id="231e6-229">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="231e6-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="231e6-230">CookieDomain</span></span>  | <span data-ttu-id="231e6-231">Il dominio del cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-231">The domain of the cookie.</span></span> <span data-ttu-id="231e6-232">Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="231e6-232">Defaults to `null`.</span></span> |
|<span data-ttu-id="231e6-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="231e6-233">CookieName</span></span>    | <span data-ttu-id="231e6-234">Il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-234">The name of the cookie.</span></span> <span data-ttu-id="231e6-235">Se non è impostata, il sistema genererà un nome univoco che inizia con il `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="231e6-235">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="231e6-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="231e6-236">CookiePath</span></span>    | <span data-ttu-id="231e6-237">Il percorso impostato nel cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-237">The path set on the cookie.</span></span> |
|<span data-ttu-id="231e6-238">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="231e6-238">FormFieldName</span></span> | <span data-ttu-id="231e6-239">Il nome del campo del form nascosto usato dal sistema non riproducibili per il rendering di un token non riproducibili nelle viste.</span><span class="sxs-lookup"><span data-stu-id="231e6-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="231e6-240">HeaderName</span><span class="sxs-lookup"><span data-stu-id="231e6-240">HeaderName</span></span>    | <span data-ttu-id="231e6-241">Il nome dell'intestazione utilizzato dal sistema non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="231e6-242">Se `null`, il sistema prenderà in considerazione solo i dati del modulo.</span><span class="sxs-lookup"><span data-stu-id="231e6-242">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="231e6-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="231e6-243">RequireSsl</span></span>    | <span data-ttu-id="231e6-244">Specifica se SSL è richiesto dal sistema non riproducibili.</span><span class="sxs-lookup"><span data-stu-id="231e6-244">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="231e6-245">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="231e6-245">Defaults to `false`.</span></span> <span data-ttu-id="231e6-246">Se `true`, le richieste non SSL avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="231e6-246">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="231e6-247">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="231e6-247">SuppressXFrameOptionsHeader</span></span> | <span data-ttu-id="231e6-248">Specifica se eliminare la generazione del `X-Frame-Options` intestazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-248">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="231e6-249">Per impostazione predefinita, viene generata l'intestazione con un valore di "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="231e6-249">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="231e6-250">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="231e6-250">Defaults to `false`.</span></span> |

<span data-ttu-id="231e6-251">Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions per altre informazioni, vedere.</span><span class="sxs-lookup"><span data-stu-id="231e6-251">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="231e6-252">Estensione Antiforgery</span><span class="sxs-lookup"><span data-stu-id="231e6-252">Extending Antiforgery</span></span>

<span data-ttu-id="231e6-253">Il [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo consente agli sviluppatori di estendere il comportamento del sistema anti-XSRF, dati aggiuntivi di andata e ritorno in ogni token.</span><span class="sxs-lookup"><span data-stu-id="231e6-253">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="231e6-254">Il [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metodo viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato.</span><span class="sxs-lookup"><span data-stu-id="231e6-254">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="231e6-255">Un responsabile dell'implementazione potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore e quindi chiamare [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) per la convalida dei dati quando il token viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="231e6-255">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="231e6-256">Nome utente del client è già incorporato nei token generati, pertanto non è necessario includere queste informazioni.</span><span class="sxs-lookup"><span data-stu-id="231e6-256">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="231e6-257">Se un token include dati supplementari, ma non `IAntiForgeryAdditionalDataProvider` è stato configurato, i dati supplementari non convalidati.</span><span class="sxs-lookup"><span data-stu-id="231e6-257">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="231e6-258">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="231e6-258">Fundamentals</span></span>

<span data-ttu-id="231e6-259">Attacchi CSRF si basano sul comportamento di invio di cookie associati a un dominio con ogni richiesta effettuata per quel dominio browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="231e6-259">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="231e6-260">Questi cookie vengono archiviati all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="231e6-260">These cookies are stored within the browser.</span></span> <span data-ttu-id="231e6-261">Spesso includono i cookie di sessione per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="231e6-261">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="231e6-262">L'autenticazione basata su cookie è una forma comune di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-262">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="231e6-263">Sistemi di autenticazione basata su token sono sempre più diffusi, soprattutto per SPA (Single Page Applications, applicazioni a pagina singola) e altri scenari "smart client".</span><span class="sxs-lookup"><span data-stu-id="231e6-263">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="231e6-264">Autenticazione basata su cookie</span><span class="sxs-lookup"><span data-stu-id="231e6-264">Cookie-based authentication</span></span>

<span data-ttu-id="231e6-265">Una volta che un utente è autenticato tramite nome utente e password, si sta rilasciati un token che può essere utilizzato per identificare in modo sicuro e convalidare che sono stati autenticati.</span><span class="sxs-lookup"><span data-stu-id="231e6-265">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="231e6-266">Il token viene memorizzato come rende un cookie associato a ogni richiesta client.</span><span class="sxs-lookup"><span data-stu-id="231e6-266">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="231e6-267">Generazione e convalida il cookie viene eseguita dal middleware di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-267">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="231e6-268">ASP.NET Core fornisce il cookie [middleware](xref:fundamentals/middleware/index) che serializza un'entità utente in un cookie crittografato e nelle richieste successive convalida quindi il cookie, ricrea l'entità e assegna al `User` proprietà `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="231e6-268">ASP.NET Core provides cookie [middleware](xref:fundamentals/middleware/index) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="231e6-269">Quando viene utilizzato un cookie, il cookie di autenticazione è semplicemente un contenitore per il ticket di autenticazione form.</span><span class="sxs-lookup"><span data-stu-id="231e6-269">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="231e6-270">Il ticket viene passato come valore del cookie di autenticazione form, con ogni richiesta e viene utilizzato dall'autenticazione basata su form, nel server, per identificare un utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="231e6-270">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="231e6-271">Quando un utente è connesso a un sistema, una sessione utente viene creata sul lato server e viene archiviata in un database o un altro archivio persistente.</span><span class="sxs-lookup"><span data-stu-id="231e6-271">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="231e6-272">Il sistema genera una chiave di sessione che punta alla sessione effettiva nell'archivio dati e viene inviato come cookie sul lato client.</span><span class="sxs-lookup"><span data-stu-id="231e6-272">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="231e6-273">Il server web verifica la chiave di sessione ogni volta che un utente richiede una risorsa che richiede l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="231e6-273">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="231e6-274">Il sistema verifica se la sessione utente associata dispone del privilegio per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="231e6-274">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="231e6-275">In questo caso, la richiesta continua.</span><span class="sxs-lookup"><span data-stu-id="231e6-275">If so, the request continues.</span></span> <span data-ttu-id="231e6-276">La richiesta in caso contrario, restituisce come non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="231e6-276">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="231e6-277">In questo approccio, i cookie vengono usati per rendere l'applicazione di stato, perché è in grado di ricordare "" che l'utente è autenticato in precedenza con il server.</span><span class="sxs-lookup"><span data-stu-id="231e6-277">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="231e6-278">Token dell'utente</span><span class="sxs-lookup"><span data-stu-id="231e6-278">User tokens</span></span>

<span data-ttu-id="231e6-279">L'autenticazione basata su token non archivia sessione sul server.</span><span class="sxs-lookup"><span data-stu-id="231e6-279">Token-based authentication doesn't store session on the server.</span></span> <span data-ttu-id="231e6-280">Quando un utente è connesso, cui è assegnati un token (non un token non riproducibili).</span><span class="sxs-lookup"><span data-stu-id="231e6-280">When a user is logged in, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="231e6-281">Il token contiene i dati necessari per convalidare il token.</span><span class="sxs-lookup"><span data-stu-id="231e6-281">This token holds the data that's required to validate the token.</span></span> <span data-ttu-id="231e6-282">Sono inoltre contenute informazioni utente sotto forma di [attestazioni](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="231e6-282">It also contains user information in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="231e6-283">Quando un utente tenta di accedere a una risorsa del server che richiede l'autenticazione, il token viene inviato al server con un'intestazione di autorizzazione aggiuntive sotto forma di connessione {token}.</span><span class="sxs-lookup"><span data-stu-id="231e6-283">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="231e6-284">In questo modo, l'applicazione senza stato poiché in ogni richiesta successiva il token viene passato nella richiesta per la convalida sul lato server.</span><span class="sxs-lookup"><span data-stu-id="231e6-284">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="231e6-285">Questo token non *crittografati*; si tratta piuttosto di *codificato*.</span><span class="sxs-lookup"><span data-stu-id="231e6-285">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="231e6-286">Sul lato server, il token può essere decodificato per accedere alle informazioni non elaborate all'interno del token.</span><span class="sxs-lookup"><span data-stu-id="231e6-286">On the server-side, the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="231e6-287">Per inviare il token nelle richieste successive, di memorizzarlo nella memoria locale del browser o in un cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-287">To send the token in subsequent requests, either store it in the browser's local storage or in a cookie.</span></span> <span data-ttu-id="231e6-288">Non preoccuparti delle vulnerabilità XSRF se il token viene archiviato nell'archiviazione locale, ma si tratta di un problema se il token viene archiviato in un cookie.</span><span class="sxs-lookup"><span data-stu-id="231e6-288">Don't worry about XSRF vulnerability if the token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="231e6-289">Più applicazioni sono ospitate in un dominio</span><span class="sxs-lookup"><span data-stu-id="231e6-289">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="231e6-290">Sebbene `example1.cloudapp.net` e `example2.cloudapp.net` sono host diversi, c'è una relazione di trust implicita tra gli host sotto il `*.cloudapp.net` dominio.</span><span class="sxs-lookup"><span data-stu-id="231e6-290">Although `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="231e6-291">Questa relazione di trust implicita consente agli host potenzialmente non attendibili determinare i cookie di altro (lo stessa origine i criteri che regolano le richieste AJAX non applicano necessariamente ai cookie HTTP).</span><span class="sxs-lookup"><span data-stu-id="231e6-291">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="231e6-292">Il runtime di ASP.NET Core fornisce alcuni attenuazione in quanto il nome utente è incorporato nel token di campo.</span><span class="sxs-lookup"><span data-stu-id="231e6-292">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token.</span></span> <span data-ttu-id="231e6-293">Anche se un sottodominio dannoso è in grado di sovrascrivere un token di sessione, non può generare un token di campo valido per l'utente.</span><span class="sxs-lookup"><span data-stu-id="231e6-293">Even if a malicious subdomain is able to overwrite a session token, it can't generate a valid field token for the user.</span></span> <span data-ttu-id="231e6-294">Quando sono ospitati in un simile ambiente, le routine di anti-XSRF incorporato ancora non è possibile difendersi Hijack della sessione o l'account di accesso CSRF attacchi.</span><span class="sxs-lookup"><span data-stu-id="231e6-294">When hosted in such an environment, the built-in anti-XSRF routines still can't defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="231e6-295">Ambienti di hosting condivisi sono vunerable Hijack della sessione, account di accesso CSRF e altri attacchi.</span><span class="sxs-lookup"><span data-stu-id="231e6-295">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="231e6-296">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="231e6-296">Additional resources</span></span>

* <span data-ttu-id="231e6-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) su [aprire il progetto di sicurezza dell'applicazione Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="231e6-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
