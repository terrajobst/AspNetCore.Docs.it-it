---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Aggiunta di Social Networking a ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come integrare il tuo sito con i social networking servizi di rete. In questo capitolo, si apprenderà come consentire agli utenti del sito Web/collegamento a segnalibro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899343"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="c8581-104">Aggiunta di Social Networking a pagine Web ASP.NET (Razor) siti</span><span class="sxs-lookup"><span data-stu-id="c8581-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="c8581-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c8581-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c8581-106">In questo articolo viene illustrato come aggiungere collegamenti di rete sociali per Facebook, Twitter, Reddit e Digg alle pagine in un sito Web ASP.NET Web Pages (Razor) e come includere i feed di Twitter, schede giocatore Xbox e Gravatar immagini.</span><span class="sxs-lookup"><span data-stu-id="c8581-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="c8581-107">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c8581-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c8581-108">Come consentire agli utenti del sito/collegamento a segnalibro.</span><span class="sxs-lookup"><span data-stu-id="c8581-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="c8581-109">Come aggiungere un feed di Twitter.</span><span class="sxs-lookup"><span data-stu-id="c8581-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="c8581-110">Come aggiungere un Facebook **come** pulsante alle pagine.</span><span class="sxs-lookup"><span data-stu-id="c8581-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="c8581-111">Come eseguire il rendering delle immagini Gravatar.com.</span><span class="sxs-lookup"><span data-stu-id="c8581-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="c8581-112">Come visualizzare una scheda giocatore Xbox sul proprio sito.</span><span class="sxs-lookup"><span data-stu-id="c8581-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c8581-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c8581-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c8581-114">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="c8581-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="c8581-115">Libreria di supporto ASP.NET Web (pacchetto NuGet)</span><span class="sxs-lookup"><span data-stu-id="c8581-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="c8581-116">In questa esercitazione si integra inoltre con ASP.NET Web Pages 3, ad eccezione di parti che utilizzano la libreria di supporto di ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="c8581-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="c8581-117">Il collegamento di sito nei siti di Social Networking</span><span class="sxs-lookup"><span data-stu-id="c8581-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="c8581-118">Se gli utenti desiderano un elemento del sito, spesso desiderano condividerlo con gli amici.</span><span class="sxs-lookup"><span data-stu-id="c8581-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="c8581-119">Si può rendere questa semplice la visualizzazione di glifi (icone) che consente di condividere una pagina su Reddit, Digg, Facebook, Twitter o siti simili.</span><span class="sxs-lookup"><span data-stu-id="c8581-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="c8581-120">Per visualizzare le icone, aggiungere il `LinkSharecode` helper a una pagina.</span><span class="sxs-lookup"><span data-stu-id="c8581-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="c8581-121">Persone, visitare la pagina è possono fare clic su un singolo glifo.</span><span class="sxs-lookup"><span data-stu-id="c8581-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="c8581-122">Se si dispone di un account con il sito di social networking, possono quindi registrare un collegamento alla pagina in tale sito.</span><span class="sxs-lookup"><span data-stu-id="c8581-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Immagine 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="c8581-124">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se sono già stati aggiunti tale - creare una pagina denominata *ListLinkShare.cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="c8581-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="c8581-125">In questo esempio, quando il `LinkShare` viene eseguito, il titolo della pagina viene passato come parametro, che a sua volta passa il titolo della pagina per il sito di social networking.</span><span class="sxs-lookup"><span data-stu-id="c8581-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="c8581-126">Tuttavia, è possibile passare qualsiasi stringa.</span><span class="sxs-lookup"><span data-stu-id="c8581-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="c8581-127">In questo esempio specifica inoltre quali siti di social networking da includere nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="c8581-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="c8581-128">È possibile specificare i siti di social networking rilevanti per il sito.</span><span class="sxs-lookup"><span data-stu-id="c8581-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="c8581-129">Eseguire il *ListLinkShare.cshtml* pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="c8581-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="c8581-130">(Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.)</span><span class="sxs-lookup"><span data-stu-id="c8581-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="c8581-131">Fare clic su un glifo per uno dei siti che sono effettuato.</span><span class="sxs-lookup"><span data-stu-id="c8581-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="c8581-132">Il collegamento reindirizza alla pagina nel sito di rete di social networking selezionata in cui è possibile condividere un collegamento.</span><span class="sxs-lookup"><span data-stu-id="c8581-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="c8581-133">Ad esempio, se si fa clic sul collegamento Reddit, è necessario per il `submit to reddit` pagina del sito Web Reddit.</span><span class="sxs-lookup"><span data-stu-id="c8581-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Immagine 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="c8581-135">Aggiunta di un Twitter Feed</span><span class="sxs-lookup"><span data-stu-id="c8581-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="c8581-136">Per informazioni sull'utilizzo di un helper di Twitter compatibile con la versione corrente dell'API di Twitter, vedere [helper di Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="c8581-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="c8581-137">In questo esempio viene illustrato come scrivere la propria helper per poterlo riutilizzare con facilità il codice dal numero di pagine.</span><span class="sxs-lookup"><span data-stu-id="c8581-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="c8581-138">Visualizzazione di un Facebook &quot;come&quot; pulsante</span><span class="sxs-lookup"><span data-stu-id="c8581-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="c8581-139">In alcuni casi, la soluzione migliore consiste nell'eseguire il codice direttamente il provider di social networking, piuttosto che basarsi su un supporto.</span><span class="sxs-lookup"><span data-stu-id="c8581-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="c8581-140">Ciò è particolarmente vero se il provider del social network Aggiorna le relative opzioni più rapidamente l'helper viene aggiornata.</span><span class="sxs-lookup"><span data-stu-id="c8581-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="c8581-141">Per aggiungere funzionalità di Facebook (ad esempio, il pulsante Like) al sito, è possibile recuperare i frammenti di codice dal [developers.facebook.com](https://developers.facebook.com/) sito.</span><span class="sxs-lookup"><span data-stu-id="c8581-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="c8581-142">Nel sito di Facebook, utilizzare gli strumenti per generare un frammento di codice che è pertinente per il sito.</span><span class="sxs-lookup"><span data-stu-id="c8581-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="c8581-143">Il codice evidenziato di seguito è il codice che è stato recuperato tramite lo strumento come pulsante sul sito developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="c8581-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="c8581-144">È necessario fornire il tuo ID app.</span><span class="sxs-lookup"><span data-stu-id="c8581-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="c8581-145">Il rendering di un'immagine Gravatar</span><span class="sxs-lookup"><span data-stu-id="c8581-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="c8581-146">Un *Gravatar* (un &quot;avatar globalmente riconosciuto&quot;) è un'immagine che può essere usata in più siti Web come proprio avatar &#8212; , ovvero un'immagine che rappresenta è.</span><span class="sxs-lookup"><span data-stu-id="c8581-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="c8581-147">Ad esempio, un Gravatar possibile identificare una persona in un post del forum, un commento, blog e così via.</span><span class="sxs-lookup"><span data-stu-id="c8581-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="c8581-148">(È possibile registrare la propria Gravatar nel sito Web Gravatar al [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Se si desidera visualizzare le immagini accanto a nomi o indirizzi di posta elettronica degli utenti nel sito Web, è possibile utilizzare l'helper Gravatar.</span><span class="sxs-lookup"><span data-stu-id="c8581-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="c8581-149">In questo esempio, si utilizza un singolo Gravatar che rappresenta se stessi.</span><span class="sxs-lookup"><span data-stu-id="c8581-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="c8581-150">Un altro modo per utilizzare un Gravatar è per consentire agli utenti di specificare l'indirizzo Gravatar quando effettuano la registrazione del sito.</span><span class="sxs-lookup"><span data-stu-id="c8581-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="c8581-151">(È possibile imparare consentire agli utenti di registrare in [sicurezza aggiunta e l'appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Quando si visualizzano informazioni per l'utente, è possibile aggiungere solo il Gravatar visualizzare dove il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c8581-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="c8581-152">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.</span><span class="sxs-lookup"><span data-stu-id="c8581-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="c8581-153">Creare una nuova pagina web denominata *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c8581-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="c8581-154">Aggiungere il markup seguente al file:</span><span class="sxs-lookup"><span data-stu-id="c8581-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="c8581-155">Il `Gravatar.GetHtml` metodo visualizza l'immagine di Gravatar nella pagina.</span><span class="sxs-lookup"><span data-stu-id="c8581-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="c8581-156">Per modificare le dimensioni dell'immagine, è possibile includere un numero come secondo parametro.</span><span class="sxs-lookup"><span data-stu-id="c8581-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="c8581-157">La dimensione predefinita è 80.</span><span class="sxs-lookup"><span data-stu-id="c8581-157">The default size is 80.</span></span> <span data-ttu-id="c8581-158">Numeri inferiore a 80 verificare l'immagine più piccoli.</span><span class="sxs-lookup"><span data-stu-id="c8581-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="c8581-159">I numeri maggiori di 80 ingrandire l'immagine.</span><span class="sxs-lookup"><span data-stu-id="c8581-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="c8581-160">Nel `Gravatar.GetHtml` metodi, sostituire `<Your Gravatar account here>` con l'indirizzo di posta elettronica utilizzato per l'account Gravatar.</span><span class="sxs-lookup"><span data-stu-id="c8581-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="c8581-161">(Se non si dispone di un account Gravatar, è possibile utilizzare l'indirizzo di posta elettronica dell'utente che esegue.)</span><span class="sxs-lookup"><span data-stu-id="c8581-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="c8581-162">Eseguire la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="c8581-162">Run the page in your browser.</span></span> <span data-ttu-id="c8581-163">Nella pagina vengono visualizzati due immagini Gravatar per l'indirizzo di posta elettronica specificato.</span><span class="sxs-lookup"><span data-stu-id="c8581-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="c8581-164">La seconda immagine è minore del primo.</span><span class="sxs-lookup"><span data-stu-id="c8581-164">The second image is smaller than the first.</span></span> 

    ![Immagine 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="c8581-166">Visualizzazione di una scheda giocatore Xbox</span><span class="sxs-lookup"><span data-stu-id="c8581-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="c8581-167">Quando gli utenti giocano Xbox Microsoft online, ogni utente dispone di un ID univoco.</span><span class="sxs-lookup"><span data-stu-id="c8581-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="c8581-168">Le statistiche vengono mantenute per ogni player sotto forma di una scheda giocatore, che mostra le reputation, punteggio giocatore e giochi di recente.</span><span class="sxs-lookup"><span data-stu-id="c8581-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="c8581-169">Se si ha un giocatore Xbox, è possibile visualizzare la scheda giocatore nelle pagine del sito utilizzando il `GamerCard` helper.</span><span class="sxs-lookup"><span data-stu-id="c8581-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="c8581-170">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.</span><span class="sxs-lookup"><span data-stu-id="c8581-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="c8581-171">Creare una nuova pagina denominata *XboxGamer.cshtml* e aggiungere il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="c8581-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="c8581-172">Utilizzare il `GamerCard.GetHtml` proprietà per specificare l'alias per la scheda giocatore da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="c8581-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="c8581-173">Eseguire la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="c8581-173">Run the page in your browser.</span></span> <span data-ttu-id="c8581-174">La pagina Visualizza la scheda giocatore Xbox specificato.</span><span class="sxs-lookup"><span data-stu-id="c8581-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Immagine 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
