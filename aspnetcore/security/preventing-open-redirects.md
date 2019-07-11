---
title: Impedire attacchi di reindirizzamento aperti in ASP.NET Core
author: ardalis
description: Viene illustrato come evitare attacchi di reindirizzamento aperti di un'app ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815496"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="3403d-103">Impedire attacchi di reindirizzamento aperti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3403d-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="3403d-104">Un'app web che reindirizza a un URL che viene specificato tramite la richiesta, ad esempio i dati della stringa di query o form potenzialmente può essere manomessi per reindirizzare gli utenti a un URL esterno, dannoso.</span><span class="sxs-lookup"><span data-stu-id="3403d-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="3403d-105">Questo manomissioni, si tratta di un attacco di reindirizzamento aperti.</span><span class="sxs-lookup"><span data-stu-id="3403d-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="3403d-106">Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non è stato manomesso.</span><span class="sxs-lookup"><span data-stu-id="3403d-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="3403d-107">ASP.NET Core ha funzionalità incorporate per proteggere le app da attacchi di reindirizzamento aperti (noto anche come aprire il reindirizzamento).</span><span class="sxs-lookup"><span data-stu-id="3403d-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="3403d-108">Che cos'è un attacco di reindirizzamento aperti?</span><span class="sxs-lookup"><span data-stu-id="3403d-108">What is an open redirect attack?</span></span>

<span data-ttu-id="3403d-109">Le applicazioni Web spesso reindirizzare gli utenti a una pagina di accesso quando accedono alle risorse che richiedono l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3403d-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="3403d-110">Il reindirizzamento in genere include un `returnUrl` parametro querystring in modo che l'utente può essere restituito all'URL richiesto originariamente dopo che hanno eseguito l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3403d-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="3403d-111">Dopo che l'utente viene autenticato, si verrà reindirizzati all'URL richiesto originariamente.</span><span class="sxs-lookup"><span data-stu-id="3403d-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="3403d-112">Poiché l'URL di destinazione è specificato nella stringa di query della richiesta, un utente malintenzionato può manomettere la stringa di query.</span><span class="sxs-lookup"><span data-stu-id="3403d-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="3403d-113">Una stringa di query alterato potrebbe consentire il sito reindirizzare l'utente a un sito esterno, dannoso.</span><span class="sxs-lookup"><span data-stu-id="3403d-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="3403d-114">Questa tecnica viene chiamata un attacco di reindirizzamento (o il reindirizzamento) aperto.</span><span class="sxs-lookup"><span data-stu-id="3403d-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="3403d-115">Un attacco di esempio</span><span class="sxs-lookup"><span data-stu-id="3403d-115">An example attack</span></span>

<span data-ttu-id="3403d-116">Un utente malintenzionato può sviluppare un attacco lo scopo di consentire all'utente malintenzionato l'accesso a credenziali o le informazioni riservate dell'utente.</span><span class="sxs-lookup"><span data-stu-id="3403d-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="3403d-117">Per avviare l'attacco, l'utente malintenzionato induce l'utente di fare clic su un collegamento alla pagina di accesso del sito con un `returnUrl` valore querystring aggiunto all'URL.</span><span class="sxs-lookup"><span data-stu-id="3403d-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="3403d-118">Si consideri ad esempio un'app all'indirizzo `contoso.com` che include una pagina di accesso a `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="3403d-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="3403d-119">L'attacco segue questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="3403d-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="3403d-120">L'utente seleziona il collegamento dannoso `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (il secondo URL è "contoso**1**. com", non "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="3403d-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="3403d-121">L'utente accede correttamente.</span><span class="sxs-lookup"><span data-stu-id="3403d-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="3403d-122">L'utente viene reindirizzato (dal sito) a `http://contoso1.com/Account/LogOn` (un sito dannoso che assomiglia esattamente a un sito vero e).</span><span class="sxs-lookup"><span data-stu-id="3403d-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="3403d-123">L'utente accede nuovamente (offrendo dannoso le proprie credenziali del sito) e viene reindirizzato al sito effettivo.</span><span class="sxs-lookup"><span data-stu-id="3403d-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="3403d-124">Probabilmente l'utente ritiene che il primo tentativo di accesso non è riuscito e che il secondo tentativo ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3403d-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="3403d-125">L'utente rimane molto probabilmente riconosce che le credenziali vengano compromesse.</span><span class="sxs-lookup"><span data-stu-id="3403d-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![Processo di attacchi di reindirizzamento aperti](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="3403d-127">Oltre alle pagine di accesso, alcuni siti forniscono pagine di reindirizzamento o gli endpoint.</span><span class="sxs-lookup"><span data-stu-id="3403d-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="3403d-128">Si supponga l'app ha una pagina con un reindirizzamento aperto, `/Home/Redirect`.</span><span class="sxs-lookup"><span data-stu-id="3403d-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="3403d-129">Un utente malintenzionato è stato possibile creare, ad esempio, un collegamento tramite posta elettronica che rimanda al `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span><span class="sxs-lookup"><span data-stu-id="3403d-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="3403d-130">Un utente tipico cercherà l'URL e vedere che inizia con il nome del sito.</span><span class="sxs-lookup"><span data-stu-id="3403d-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="3403d-131">Concessione dell'attendibilità che, essi verranno fare clic sul collegamento.</span><span class="sxs-lookup"><span data-stu-id="3403d-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="3403d-132">Il reindirizzamento aperto invierebbe quindi l'utente al sito di phishing, il cui aspetto è identico a quella dell'utente, e l'utente potrebbe accedere a ciò che si ritiene che è il sito.</span><span class="sxs-lookup"><span data-stu-id="3403d-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="3403d-133">La protezione da attacchi di reindirizzamento aperti</span><span class="sxs-lookup"><span data-stu-id="3403d-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="3403d-134">Durante lo sviluppo di applicazioni web, considera tutti i dati forniti dall'utente come non attendibili.</span><span class="sxs-lookup"><span data-stu-id="3403d-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="3403d-135">Se l'applicazione include funzionalità che reindirizza l'utente in base al contenuto dell'URL, verificare che questi reindirizzamenti vengono eseguiti solo in locale all'interno dell'app (o a un URL noto, non qualsiasi URL che può essere fornito nella stringa di query).</span><span class="sxs-lookup"><span data-stu-id="3403d-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="3403d-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="3403d-136">LocalRedirect</span></span>

<span data-ttu-id="3403d-137">Usare la `LocalRedirect` metodo di supporto dalla base `Controller` classe:</span><span class="sxs-lookup"><span data-stu-id="3403d-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="3403d-138">`LocalRedirect` verrà generata un'eccezione se viene specificato un URL non locali.</span><span class="sxs-lookup"><span data-stu-id="3403d-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="3403d-139">In caso contrario, si comporta esattamente come il `Redirect` (metodo).</span><span class="sxs-lookup"><span data-stu-id="3403d-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="3403d-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="3403d-140">IsLocalUrl</span></span>

<span data-ttu-id="3403d-141">Usare la [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodo per testare gli URL prima di reindirizzamento:</span><span class="sxs-lookup"><span data-stu-id="3403d-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="3403d-142">Nell'esempio seguente viene illustrato come controllare se un URL è locale prima del reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="3403d-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="3403d-143">Il `IsLocalUrl` metodo protegge gli utenti non venga inavvertitamente reindirizzato a un sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="3403d-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="3403d-144">È possibile registrare i dettagli dell'URL che è stato fornito quando viene fornito un URL non locali in una situazione in cui previsto un URL locale.</span><span class="sxs-lookup"><span data-stu-id="3403d-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="3403d-145">La registrazione di reindirizzamento URL può essere utile per diagnosticare gli attacchi di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="3403d-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
