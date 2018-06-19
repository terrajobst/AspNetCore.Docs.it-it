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
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Aggiunta di Social Networking a pagine Web ASP.NET (Razor) siti
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come aggiungere collegamenti di rete sociali per Facebook, Twitter, Reddit e Digg alle pagine in un sito Web ASP.NET Web Pages (Razor) e come includere i feed di Twitter, schede giocatore Xbox e Gravatar immagini.
> 
> Illustra quanto segue:
> 
> - Come consentire agli utenti del sito/collegamento a segnalibro.
> - Come aggiungere un feed di Twitter.
> - Come aggiungere un Facebook **come** pulsante alle pagine.
> - Come eseguire il rendering delle immagini Gravatar.com.
> - Come visualizzare una scheda giocatore Xbox sul proprio sito.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - Libreria di supporto ASP.NET Web (pacchetto NuGet)
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 3, ad eccezione di parti che utilizzano la libreria di supporto di ASP.NET Web.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Il collegamento di sito nei siti di Social Networking

Se gli utenti desiderano un elemento del sito, spesso desiderano condividerlo con gli amici. Si può rendere questa semplice la visualizzazione di glifi (icone) che consente di condividere una pagina su Reddit, Digg, Facebook, Twitter o siti simili.

Per visualizzare le icone, aggiungere il `LinkSharecode` helper a una pagina. Persone, visitare la pagina è possono fare clic su un singolo glifo. Se si dispone di un account con il sito di social networking, possono quindi registrare un collegamento alla pagina in tale sito.

![Immagine 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se sono già stati aggiunti tale - creare una pagina denominata *ListLinkShare.cshtml* e aggiungere il markup seguente:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    In questo esempio, quando il `LinkShare` viene eseguito, il titolo della pagina viene passato come parametro, che a sua volta passa il titolo della pagina per il sito di social networking. Tuttavia, è possibile passare qualsiasi stringa. In questo esempio specifica inoltre quali siti di social networking da includere nell'elenco. È possibile specificare i siti di social networking rilevanti per il sito.
2. Eseguire il *ListLinkShare.cshtml* pagina in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.)
3. Fare clic su un glifo per uno dei siti che sono effettuato. Il collegamento reindirizza alla pagina nel sito di rete di social networking selezionata in cui è possibile condividere un collegamento. Ad esempio, se si fa clic sul collegamento Reddit, è necessario per il `submit to reddit` pagina del sito Web Reddit.

     ![Immagine 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Aggiunta di un Twitter Feed

Per informazioni sull'utilizzo di un helper di Twitter compatibile con la versione corrente dell'API di Twitter, vedere [helper di Twitter](../ui-layouts-and-themes/twitter-helper.md). In questo esempio viene illustrato come scrivere la propria helper per poterlo riutilizzare con facilità il codice dal numero di pagine.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Visualizzazione di un Facebook &quot;come&quot; pulsante

In alcuni casi, la soluzione migliore consiste nell'eseguire il codice direttamente il provider di social networking, piuttosto che basarsi su un supporto. Ciò è particolarmente vero se il provider del social network Aggiorna le relative opzioni più rapidamente l'helper viene aggiornata.

Per aggiungere funzionalità di Facebook (ad esempio, il pulsante Like) al sito, è possibile recuperare i frammenti di codice dal [developers.facebook.com](https://developers.facebook.com/) sito. Nel sito di Facebook, utilizzare gli strumenti per generare un frammento di codice che è pertinente per il sito.

Il codice evidenziato di seguito è il codice che è stato recuperato tramite lo strumento come pulsante sul sito developers.facebook.com. È necessario fornire il tuo ID app.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Il rendering di un'immagine Gravatar

Un *Gravatar* (un &quot;avatar globalmente riconosciuto&quot;) è un'immagine che può essere usata in più siti Web come proprio avatar &#8212; , ovvero un'immagine che rappresenta è. Ad esempio, un Gravatar possibile identificare una persona in un post del forum, un commento, blog e così via. (È possibile registrare la propria Gravatar nel sito Web Gravatar al [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Se si desidera visualizzare le immagini accanto a nomi o indirizzi di posta elettronica degli utenti nel sito Web, è possibile utilizzare l'helper Gravatar.

In questo esempio, si utilizza un singolo Gravatar che rappresenta se stessi. Un altro modo per utilizzare un Gravatar è per consentire agli utenti di specificare l'indirizzo Gravatar quando effettuano la registrazione del sito. (È possibile imparare consentire agli utenti di registrare in [sicurezza aggiunta e l'appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Quando si visualizzano informazioni per l'utente, è possibile aggiungere solo il Gravatar visualizzare dove il nome dell'utente.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
2. Creare una nuova pagina web denominata *Gravatar.cshtml*.
3. Aggiungere il markup seguente al file: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Il `Gravatar.GetHtml` metodo visualizza l'immagine di Gravatar nella pagina. Per modificare le dimensioni dell'immagine, è possibile includere un numero come secondo parametro. La dimensione predefinita è 80. Numeri inferiore a 80 verificare l'immagine più piccoli. I numeri maggiori di 80 ingrandire l'immagine.
4. Nel `Gravatar.GetHtml` metodi, sostituire `<Your Gravatar account here>` con l'indirizzo di posta elettronica utilizzato per l'account Gravatar. (Se non si dispone di un account Gravatar, è possibile utilizzare l'indirizzo di posta elettronica dell'utente che esegue.)
5. Eseguire la pagina nel browser. Nella pagina vengono visualizzati due immagini Gravatar per l'indirizzo di posta elettronica specificato. La seconda immagine è minore del primo. 

    ![Immagine 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Visualizzazione di una scheda giocatore Xbox

Quando gli utenti giocano Xbox Microsoft online, ogni utente dispone di un ID univoco. Le statistiche vengono mantenute per ogni player sotto forma di una scheda giocatore, che mostra le reputation, punteggio giocatore e giochi di recente. Se si ha un giocatore Xbox, è possibile visualizzare la scheda giocatore nelle pagine del sito utilizzando il `GamerCard` helper.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
2. Creare una nuova pagina denominata *XboxGamer.cshtml* e aggiungere il markup seguente.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Utilizzare il `GamerCard.GetHtml` proprietà per specificare l'alias per la scheda giocatore da visualizzare.
3. Eseguire la pagina nel browser. La pagina Visualizza la scheda giocatore Xbox specificato.

    ![Immagine 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
