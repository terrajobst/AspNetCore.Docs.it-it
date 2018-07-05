---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Sito utilizzando un CAPTCHA per impedire l'uso di Razor di ASP.NET Web Bot) | Microsoft Docs
author: microsoft
description: Questo articolo illustra come usare ReCaptcha (una misura di sicurezza) per impedire l'esecuzione di attività in un ASP.NET Web Pages (Razor) i programmi automatizzati (Bot) abbiamo...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803071"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="a5e2b-103">Sito utilizzando un CAPTCHA per impedire l'uso di Razor di ASP.NET Web Bot)</span><span class="sxs-lookup"><span data-stu-id="a5e2b-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="a5e2b-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a5e2b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a5e2b-105">Questo articolo illustra come usare ReCaptcha (una misura di sicurezza) per impedire l'esecuzione di attività in un sito Web di ASP.NET Web Pages (Razor) i programmi automatizzati (Bot).</span><span class="sxs-lookup"><span data-stu-id="a5e2b-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="a5e2b-106">**Che cosa si apprenderà come:**</span><span class="sxs-lookup"><span data-stu-id="a5e2b-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="a5e2b-107">Come aggiungere un test CAPTCHA al sito.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="a5e2b-108">Queste sono le funzionalità ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="a5e2b-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="a5e2b-109">Il `ReCaptcha` helper.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="a5e2b-110">Le informazioni contenute in questo articolo si applicano a 1.0 di pagine Web ASP.NET e Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="a5e2b-111">Sulle CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="a5e2b-111">About CAPTCHAs</span></span>

<span data-ttu-id="a5e2b-112">Ogni volta che si consentono di registrare nel sito, o anche semplicemente immettere un nome e un URL (ad esempio per un commento di blog), si potrebbe ricevere un flusso eccessivo di specificando nomi falsi.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="a5e2b-113">Spesso, questi vengono lasciati da programmi automatizzati (Bot) che tentano di mantenere gli URL in ogni sito Web che sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="a5e2b-114">(Una motivazione comune consiste nel registrare l'URL dei prodotti in vendita).</span><span class="sxs-lookup"><span data-stu-id="a5e2b-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="a5e2b-115">Si può assicurare che un utente sia persona reale e non un programma con un *CAPTCHA* per convalidare gli utenti quando si registra o in caso contrario, immettere il nome e il sito.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="a5e2b-116">CAPTCHA è l'acronimo di test completamente automatizzata pubblico Turing indicare i computer e gli esseri umani distanti.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="a5e2b-117">Un CAPTCHA è un *challenge / response* test in cui l'utente viene richiesto di eseguire un'operazione facile per un utente a eseguire operazioni ma rigido per un programma automatico eseguire.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="a5e2b-118">Il tipo più comune di CAPTCHA è uno in cui si vedere alcune lettere distorta e viene richiesto di digitarli.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="a5e2b-119">(La distorsione dovrebbe per risultare difficile per Bot possono essere quindi decifrate le lettere.)</span><span class="sxs-lookup"><span data-stu-id="a5e2b-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="a5e2b-120">Aggiunta di un Test di ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="a5e2b-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="a5e2b-121">Nelle pagine ASP.NET, è possibile usare la `ReCaptcha` helper per eseguire il rendering di un test CAPTCHA che si basa sul servizio ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="a5e2b-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="a5e2b-122">Il `ReCaptcha` helper Visualizza un'immagine di due parole distorte che gli utenti devono immettere correttamente prima che la pagina viene convalidata.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="a5e2b-123">La risposta dell'utente viene convalidata dal servizio Recaptcha.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="a5e2b-124">Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="a5e2b-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="a5e2b-125">Dopo aver completato la registrazione, si otterrà una chiave pubblica e una chiave privata.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="a5e2b-126">Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="a5e2b-127">Se si ha già un  *\_AppStart.cshtml* file, nella cartella radice di un sito Web, creare un file denominato  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="a5e2b-128">Aggiungere il codice seguente `Recaptcha` le impostazioni di helper nel  *\_AppStart.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="a5e2b-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="a5e2b-129">Impostare il `PublicKey` e `PrivateKey` proprietà mediante le proprie chiavi pubbliche e private.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="a5e2b-130">Salvare il  *\_AppStart.cshtml* file e chiuderlo.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="a5e2b-131">Nella cartella radice di un sito Web, creare nuova pagina denominata *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="a5e2b-132">Sostituire il contenuto esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a5e2b-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="a5e2b-133">Eseguire la *Recaptcha.cshtml* pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="a5e2b-134">Se il `PrivateKey` valore sia valido, la pagina Visualizza il controllo ReCaptcha e un pulsante.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="a5e2b-135">Se non è stata impostata a livello globale in quelle  *\_AppStart.html*, la pagina verrà visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="a5e2b-136">Immettere le parole per il test.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-136">Enter the words for the test.</span></span> <span data-ttu-id="a5e2b-137">Se si passa il test ReCaptcha, viene visualizzato un messaggio in tal senso.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="a5e2b-138">In caso contrario, viene visualizzato un messaggio di errore e viene visualizzata di nuovo il controllo di ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="a5e2b-139">Se il computer si trova in un dominio che utilizza un server proxy, si potrebbe essere necessario configurare il `defaultproxy` dell'elemento il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="a5e2b-140">L'esempio seguente mostra una *Web. config* file con il `defaultproxy` elemento configurato per consentire il funzionamento del servizio ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="a5e2b-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a5e2b-141">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a5e2b-141">Additional Resources</span></span>


- [<span data-ttu-id="a5e2b-142">Personalizzazione del comportamento a livello di sito per siti con pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a5e2b-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="a5e2b-143">Sito ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="a5e2b-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
