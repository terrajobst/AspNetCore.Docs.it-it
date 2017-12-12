---
title: Prevenzione degli attacchi di reindirizzamento aperto in un'applicazione ASP.NET Core | Documenti Microsoft
author: ardalis
description: Viene illustrato come evitare attacchi di reindirizzamento aprire un'applicazione ASP.NET di base
keywords: Attacchi di reindirizzamento aprire ASP.NET Core, sicurezza,
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 4083845a77eb19d9ba9beb389a92ceb5c14edbde
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="b8c3d-104">Prevenzione degli attacchi di reindirizzamento aperto in un'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="b8c3d-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="b8c3d-105">Un'app web che reindirizza a un URL che viene specificato tramite la richiesta, ad esempio i dati di stringa di query o i form può essere alterata potenzialmente per reindirizzare gli utenti a un URL esterno, dannoso.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="b8c3d-106">Questo tipo di manomissione viene chiamato un attacco di reindirizzamento aperto.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="b8c3d-107">Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non è stato alterato.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="b8c3d-108">ASP.NET Core è una funzionalità integrata per proteggere le app da attacchi di reindirizzamento aprire (anche noto come open reindirizzamento).</span><span class="sxs-lookup"><span data-stu-id="b8c3d-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="b8c3d-109">Che cos'è un attacco di reindirizzamento aprire?</span><span class="sxs-lookup"><span data-stu-id="b8c3d-109">What is an open redirect attack?</span></span>

<span data-ttu-id="b8c3d-110">Applicazioni Web spesso reindirizzare gli utenti a una pagina di accesso quando accedono alle risorse che richiedono l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="b8c3d-111">Il reindirizzamento typlically include un `returnUrl` parametro querystring in modo che l'utente può essere restituito all'URL richiesto originariamente dopo che hanno effettuato correttamente l'accesso.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="b8c3d-112">Dopo aver autenticato l'utente, viene reindirizzato all'URL richiesto originariamente.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="b8c3d-113">Poiché l'URL di destinazione è specificato nella stringa di query della richiesta, un utente malintenzionato può manomettere il parametro querystring.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="b8c3d-114">Una stringa di query manomessi potrebbe consentire al sito di reindirizzare l'utente a un sito esterno, dannoso.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="b8c3d-115">Questa tecnica viene chiamata un attacco di reindirizzamento (o di reindirizzamento) aperto.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="b8c3d-116">Un attacco di esempio</span><span class="sxs-lookup"><span data-stu-id="b8c3d-116">An example attack</span></span>

<span data-ttu-id="b8c3d-117">Un utente malintenzionato potrebbe sviluppare un attacco che consente l'accesso utente non autorizzato a informazioni riservate sull'applicazione o le credenziali di un utente.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="b8c3d-118">Per avviare l'attacco, essi convincere l'utente di fare clic su un collegamento alla pagina di accesso del sito, con un `returnUrl` valore querystring aggiunto all'URL.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="b8c3d-119">Ad esempio, il [NerdDinner.com](http://nerddinner.com) , scritto per ASP.NET MVC, l'applicazione di esempio include tali una pagina di accesso qui: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="b8c3d-120">L'attacco quindi questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="b8c3d-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="b8c3d-121">Utente fa clic su un collegamento a ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (si noti che secondo URL è nerddi**n**effettivamente non nerddi**nn**er).</span><span class="sxs-lookup"><span data-stu-id="b8c3d-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="b8c3d-122">L'utente accede correttamente.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="b8c3d-123">L'utente viene reindirizzato (per il sito) ``http://nerddiner.com/Account/LogOn`` (sito dannoso simile sito reale).</span><span class="sxs-lookup"><span data-stu-id="b8c3d-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="b8c3d-124">L'utente accede nuovamente (dando dannoso le proprie credenziali del sito) e viene reindirizzato al sito effettivo.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="b8c3d-125">L'utente verrà probabilmente ritiene che il primo tentativo di accedere non è riuscita e il secondo è stata eseguita correttamente.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="b8c3d-126">Sarà probabilmente rimangono anche senza conoscere le credenziali sono stati compromessi.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Processo di attacchi di reindirizzamento aperto](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="b8c3d-128">Oltre alle pagine di accesso, alcuni siti forniscono pagine di reindirizzamento o di endpoint.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="b8c3d-129">Si supponga l'app abbia una pagina con un reindirizzamento di aprire, ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="b8c3d-130">Un utente malintenzionato potrebbe creare ad esempio, un collegamento tramite posta elettronica a ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="b8c3d-131">Un utente tipico analizza l'URL e vedere che inizia con il nome del sito.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="b8c3d-132">Concessione dell'attendibilità che, essi verranno fare clic sul collegamento.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="b8c3d-133">Il reindirizzamento aprire invierebbe quindi l'utente al sito di phishing, identico al proprio, e l'utente potrebbe accedere a essi ritiene è il sito.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="b8c3d-134">Protezione da attacchi di reindirizzamento aperto</span><span class="sxs-lookup"><span data-stu-id="b8c3d-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="b8c3d-135">Quando si sviluppano applicazioni web, considerare tutti i dati forniti dall'utente come non attendibili.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="b8c3d-136">Se l'applicazione dispone di funzionalità che reindirizza l'utente in base al contenuto dell'URL, assicurarsi che tali reindirizzamenti sono solo in locale all'interno dell'app (o a un URL noto, non qualsiasi URL che può essere fornito nella stringa di query).</span><span class="sxs-lookup"><span data-stu-id="b8c3d-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="b8c3d-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="b8c3d-137">LocalRedirect</span></span>

<span data-ttu-id="b8c3d-138">Utilizzare il ``LocalRedirect`` metodo helper dalla base `Controller` classe:</span><span class="sxs-lookup"><span data-stu-id="b8c3d-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="b8c3d-139">``LocalRedirect``verrà generata un'eccezione se viene specificato un URL non locali.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="b8c3d-140">In caso contrario, si comporta esattamente come il ``Redirect`` metodo.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="b8c3d-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="b8c3d-141">IsLocalUrl</span></span>

<span data-ttu-id="b8c3d-142">Utilizzare il [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodo per testare gli URL prima di reindirizzamento:</span><span class="sxs-lookup"><span data-stu-id="b8c3d-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="b8c3d-143">Nell'esempio seguente viene illustrato come controllare se un URL è locale prima di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="b8c3d-144">Il `IsLocalUrl` metodo protegge gli utenti non venga inavvertitamente reindirizzato a un sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="b8c3d-145">È possibile registrare i dettagli dell'URL che è stato specificato quando viene fornito un URL non locali in una situazione in cui previsto un URL locale.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="b8c3d-146">Registrazione di reindirizzamento URL può essere utile per diagnosticare gli attacchi di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="b8c3d-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
