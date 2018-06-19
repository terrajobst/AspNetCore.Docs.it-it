---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Informazioni relative ai visitatori (Analitica) di rilevamento per un ASP.NET Web Pages sito (Razor) | Documenti Microsoft
author: tfitzmac
description: Dopo è stato utilizzato il sito Web prevede, è possibile analizzare il traffico del sito Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528760"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="6e68c-103">Informazioni di rilevamento visitatore (Analitica) per un sito Web di ASP.NET di pagine (Razor)</span><span class="sxs-lookup"><span data-stu-id="6e68c-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="6e68c-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6e68c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6e68c-105">In questo articolo viene illustrato come utilizzare un file di supporto per aggiungere analitica sito Web alle pagine in un sito Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="6e68c-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="6e68c-106">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="6e68c-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="6e68c-107">Come inviare il traffico del sito Web di informazioni a un provider analitica.</span><span class="sxs-lookup"><span data-stu-id="6e68c-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="6e68c-108">Si tratta di programmazione delle funzionalità introdotte nell'articolo ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6e68c-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="6e68c-109">Il `Analytics` helper.</span><span class="sxs-lookup"><span data-stu-id="6e68c-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6e68c-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6e68c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6e68c-111">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="6e68c-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="6e68c-112">ASP.NET Web Helpers Library (pacchetto NuGet)</span><span class="sxs-lookup"><span data-stu-id="6e68c-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="6e68c-113">Analitica è un termine generale per la tecnologia che misura il traffico nel sito Web in modo è possibile comprendere la modalità di utilizzo del sito.</span><span class="sxs-lookup"><span data-stu-id="6e68c-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="6e68c-114">Molti servizi analitica sono disponibili, inclusi i servizi da Google, Yahoo, StatCounter e altri utenti.</span><span class="sxs-lookup"><span data-stu-id="6e68c-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="6e68c-115">Analitica il modo works è che si esegue l'iscrizione per un account con il provider analitica, in cui si registra il sito che si desidera tenere traccia. Il provider invia un frammento di codice JavaScript che include un ID o il rilevamento del codice per l'account.</span><span class="sxs-lookup"><span data-stu-id="6e68c-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="6e68c-116">Aggiungere il frammento di codice JavaScript per iniziare a utilizzare le pagine web per il sito che si desidera tenere traccia. (Si aggiunge in genere il frammento analitica a una pagina di piè di pagina o il layout o altro markup HTML che viene visualizzato in ogni pagina del sito.) Quando gli utenti richiedono una pagina contenente uno di questi frammenti di codice JavaScript, il frammento di codice invia informazioni relative alla pagina corrente per il provider analitica, che registra diversi dettagli sulla pagina.</span><span class="sxs-lookup"><span data-stu-id="6e68c-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="6e68c-117">Quando si desidera esaminare le statistiche del sito, accedere al sito Web del provider analitica.</span><span class="sxs-lookup"><span data-stu-id="6e68c-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="6e68c-118">È quindi possibile visualizzare tutti i tipi di report sul sito, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6e68c-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="6e68c-119">Il numero di visualizzazioni di pagina per le singole pagine.</span><span class="sxs-lookup"><span data-stu-id="6e68c-119">The number of page views for individual pages.</span></span> <span data-ttu-id="6e68c-120">In questo modo si (approssimativamente) quante persone hanno visita il sito e le pagine del sito sono i più diffusi.</span><span class="sxs-lookup"><span data-stu-id="6e68c-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="6e68c-121">Il tempo dedicato alle attività in pagine specifiche.</span><span class="sxs-lookup"><span data-stu-id="6e68c-121">How long people spend on specific pages.</span></span> <span data-ttu-id="6e68c-122">Questo può indicare ad esempio se la home page è mantenendo interesse degli utenti.</span><span class="sxs-lookup"><span data-stu-id="6e68c-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="6e68c-123">Le persone siti presenti prima che visita il sito.</span><span class="sxs-lookup"><span data-stu-id="6e68c-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="6e68c-124">Questo aiuta a capire se il traffico in ingresso dai collegamenti, da ricerche e così via.</span><span class="sxs-lookup"><span data-stu-id="6e68c-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="6e68c-125">Quando gli utenti visitano il sito e tempi di permanenza.</span><span class="sxs-lookup"><span data-stu-id="6e68c-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="6e68c-126">In quali paesi sono compresi tra i visitatori.</span><span class="sxs-lookup"><span data-stu-id="6e68c-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="6e68c-127">Il browser e sistemi operativi utilizza i visitatori.</span><span class="sxs-lookup"><span data-stu-id="6e68c-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="6e68c-129">Utilizzo di un Helper per aggiungere Analitica a una pagina</span><span class="sxs-lookup"><span data-stu-id="6e68c-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="6e68c-130">Pagine Web ASP.NET include diversi helper analitica (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) che rendono più semplice gestire i frammenti di codice JavaScript utilizzati per analitica.</span><span class="sxs-lookup"><span data-stu-id="6e68c-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="6e68c-131">Anziché pensare a come e dove inserire il codice JavaScript, è necessario effettuare è aggiungere il supporto a una pagina.</span><span class="sxs-lookup"><span data-stu-id="6e68c-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="6e68c-132">L'unica informazione che è necessario fornire è il nome dell'account, un ID o un codice di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="6e68c-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="6e68c-133">(Per StatCounter, è inoltre necessario fornire alcuni valori aggiuntivi.)</span><span class="sxs-lookup"><span data-stu-id="6e68c-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="6e68c-134">In questa procedura, si creerà una pagina di layout che utilizza il `GetGoogleHtml` helper.</span><span class="sxs-lookup"><span data-stu-id="6e68c-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="6e68c-135">Se si dispone già di un account con uno degli altri provider analitica, è possibile utilizzare tale account e apportare piccole modifiche alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="6e68c-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="6e68c-136">Quando si crea un account analitica, registrare l'URL del sito che si desidera essere rilevamento.</span><span class="sxs-lookup"><span data-stu-id="6e68c-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="6e68c-137">Se si sta testando tutti gli elementi presenti nel computer locale, non sarà necessario registrare traffico effettivo (il solo traffico è è), in modo non sarà in grado di registrare e visualizzare le statistiche del sito.</span><span class="sxs-lookup"><span data-stu-id="6e68c-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="6e68c-138">Ma questa procedura viene illustrato come aggiungere un helper analitica a una pagina.</span><span class="sxs-lookup"><span data-stu-id="6e68c-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="6e68c-139">Quando si pubblica il sito, il sito in tempo reale invia informazioni al provider analitica.</span><span class="sxs-lookup"><span data-stu-id="6e68c-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="6e68c-140">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.</span><span class="sxs-lookup"><span data-stu-id="6e68c-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="6e68c-141">Creare un account con Google Analitica e registrare il nome dell'account.</span><span class="sxs-lookup"><span data-stu-id="6e68c-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="6e68c-142">Creare una pagina di layout denominata *Analytics.cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="6e68c-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="6e68c-143">È necessario inserire la chiamata al `Analytics` helper nel corpo della pagina web (prima di `</body>` tag).</span><span class="sxs-lookup"><span data-stu-id="6e68c-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="6e68c-144">In caso contrario, il browser non eseguirà lo script.</span><span class="sxs-lookup"><span data-stu-id="6e68c-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="6e68c-145">Se si utilizza un provider diverso analitica, utilizzare uno degli helper seguenti:</span><span class="sxs-lookup"><span data-stu-id="6e68c-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="6e68c-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="6e68c-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="6e68c-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="6e68c-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="6e68c-148">Sostituire `myaccount` con il nome dell'account, ID o codice di rilevamento creato nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="6e68c-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="6e68c-149">Eseguire la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="6e68c-149">Run the page in the browser.</span></span> <span data-ttu-id="6e68c-150">(Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.)</span><span class="sxs-lookup"><span data-stu-id="6e68c-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="6e68c-151">Nel browser, visualizzare l'origine della pagina.</span><span class="sxs-lookup"><span data-stu-id="6e68c-151">In the browser, view the page source.</span></span> <span data-ttu-id="6e68c-152">Sarà in grado di visualizzare il codice sottoposto a rendering analitica:</span><span class="sxs-lookup"><span data-stu-id="6e68c-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="6e68c-153">Accedere al sito di Google Analitica ed esaminare le statistiche per il sito.</span><span class="sxs-lookup"><span data-stu-id="6e68c-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="6e68c-154">Se si esegue la pagina in un sito in tempo reale, è visualizzata una voce che registra la visita a una pagina.</span><span class="sxs-lookup"><span data-stu-id="6e68c-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6e68c-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6e68c-155">Additional Resources</span></span>

- [<span data-ttu-id="6e68c-156">Sito di Google Analitica</span><span class="sxs-lookup"><span data-stu-id="6e68c-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="6e68c-157">Yahoo! Analitica sito Web</span><span class="sxs-lookup"><span data-stu-id="6e68c-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="6e68c-158">Sito StatCounter</span><span class="sxs-lookup"><span data-stu-id="6e68c-158">StatCounter site</span></span>](http://statcounter.com/)
