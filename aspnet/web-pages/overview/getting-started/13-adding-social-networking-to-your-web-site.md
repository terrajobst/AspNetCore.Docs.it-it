---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Aggiunta di Social Networking per ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: In questo capitolo illustra come integrare il sito con servizi di social networking. In questo capitolo, si apprenderà come permettere alle persone/il sito Web di collegamento a segnalibro...
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: e50a35d9770da247d18bbe1b3660b7bd5d46d8e9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822444"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Aggiunta di social networking siti di rete alle pagine Web ASP.NET (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come aggiungere collegamenti di rete basati su social network per Facebook, Twitter, Reddit e Digg alle pagine in un sito Web ASP.NET Web Pages (Razor) e come includere immagini Gravatar feed di Twitter e le schede del giocatore di Xbox.
> 
> Che cosa si apprenderà come:
> 
> - Come consentire agli utenti del sito/collegamento a segnalibro.
> - Come aggiungere un feed di Twitter.
> - Come aggiungere un Facebook **, ad esempio** pulsante alle pagine.
> - Come eseguire il rendering di immagini Gravatar.com.
> - Come visualizzare una scheda del giocatore di Xbox sul proprio sito.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Libreria Helper ASP.NET Web (pacchetto NuGet)
>   
> 
> Questa esercitazione funziona anche con 3 pagine Web ASP.NET, ad eccezione di parti che usano la libreria Helper Web di ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Collegamento di sito Web in siti di Social Networking

Se gli utenti desiderano qualcosa nel sito, spesso desiderano condividerlo con gli amici. È possibile facilitare questa operazione mediante la visualizzazione di glifi (icone) che gli utenti potranno selezionare per condividere una pagina su Digg, Reddit, Facebook, Twitter o siti simili.

Per visualizzare le icone, aggiungere il `LinkSharecode` helper a una pagina. Gli utenti che visitano la pagina è possono fare clic su un singolo glifo. Se si dispone di un account con il sito di social networking, possono quindi registrare un collegamento alla pagina nel sito in questione.

![Immagine 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se è stata già aggiunta utente - creare una pagina denominata *ListLinkShare.cshtml* e aggiungere il markup seguente:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    In questo esempio, quando il `LinkShare` viene eseguito, il titolo della pagina viene passato come parametro, che a sua volta inoltra il titolo della pagina per il sito di social networking. Tuttavia, è possibile passare in qualsiasi stringa desiderata. In questo esempio specifica inoltre quali siti di social networking da includere nell'elenco. È possibile specificare i siti di social networking rilevanti per il sito.
2. Eseguire la *ListLinkShare.cshtml* pagina in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.)
3. Fare clic su un'icona per uno dei siti che è effettuato l'iscrizione. Il collegamento reindirizza alla pagina nel sito di rete basati su social network selezionata in cui è possibile condividere un collegamento. Ad esempio, se si fa clic sul collegamento Reddit, si verrà reindirizzati al `submit to reddit` pagina sul sito Web Reddit.

     ![Immagine 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Aggiunta di un Twitter Feed

Per informazioni sull'uso di un helper di Twitter che è compatibile con la versione corrente dell'API di Twitter, vedere [helper di Twitter](../ui-layouts-and-themes/twitter-helper.md). In questo esempio viene illustrato come scrivere il proprio supporto per poterlo riutilizzare facilmente il codice dal numero di pagine.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Visualizzazione di un Facebook &quot;, ad esempio&quot; pulsante

In alcuni casi, la scelta migliore consiste nell'eseguire il codice direttamente da provider di rete basati su social network anziché basarsi su un file di supporto. Ciò è particolarmente vero se il provider di social network Aggiorna le relative opzioni più veloce l'helper è aggiornato.

Per aggiungere funzionalità di Facebook (ad esempio, il pulsante Like) nel sito, è possibile recuperare i frammenti di codice dal [developers.facebook.com](https://developers.facebook.com/) sito. Nel sito di Facebook, utilizzare gli strumenti per generare un frammento di codice che è rilevante per il sito.

Il codice evidenziato seguente è il codice che è stato recuperato dallo strumento, ad esempio pulsante nel sito in developers.facebook.com. È necessario fornire il proprio ID app.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Il rendering di un'immagine Gravatar

Oggetto *Gravatar* (un &quot;avatar riconosciuto a livello globale&quot;) è un'immagine che può essere utilizzata in più siti Web come tuo avatar &#8212; , ovvero un'immagine che rappresenta è. Ad esempio, un Gravatar possibile identificare una persona in un post del forum, in un commento, blog e così via. (È possibile registrare il proprio Gravatar presso il sito Web Gravatar alla [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Se si desidera visualizzare le immagini accanto ai nomi o indirizzi di posta elettronica delle persone nel tuo sito Web, è possibile usare l'helper Gravatar.

In questo esempio si usa un singolo Gravatar che rappresenta se stessi. Un altro modo per usare un Gravatar è permettere alle persone di specificare il proprio indirizzo Gravatar quando vengono registrati nel sito. (È possibile imparare a consentire agli utenti di registrare in [aggiunta di sicurezza e l'appartenenza a un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Quando si visualizzano informazioni per quell'utente, è possibile aggiungere semplicemente il Gravatar visualizzare dove il nome dell'utente.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
2. Creare una nuova pagina web denominata *Gravatar.cshtml*.
3. Aggiungere il markup seguente al file: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Il `Gravatar.GetHtml` metodo visualizza l'immagine Gravatar nella pagina. Per modificare le dimensioni dell'immagine, è possibile includere un numero come secondo parametro. La dimensione predefinita è 80. Numeri di rendere meno di 80 l'immagine più piccoli. I numeri maggiori di 80 ingrandire l'immagine.
4. Nel `Gravatar.GetHtml` metodi, sostituire `<Your Gravatar account here>` con l'indirizzo di posta elettronica usato per l'account Gravatar. (Se non hai un account Gravatar, è possibile utilizzare l'indirizzo di posta elettronica di un utente che esegue.)
5. Eseguire la pagina nel browser. La pagina Visualizza immagini Gravatar due per l'indirizzo di posta elettronica specificato. La seconda immagine è inferiore rispetto alla prima. 

    ![Immagine 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Visualizzazione di una scheda del giocatore di Xbox

Quando Giochiamo Xbox Microsoft online, ogni utente dispone di un ID univoco. Le statistiche vengono mantenute per ognuno di essi in forma di una scheda del giocatore, che mostra la reputazione, punteggio giocatore e recentemente i giochi. Se sei un giocatore di Xbox, è possibile visualizzare la scheda dei giocatori in pagine nel sito usando il `GamerCard` helper.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
2. Creare una nuova pagina denominata *XboxGamer.cshtml* e aggiungere il markup seguente.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Si utilizza il `GamerCard.GetHtml` proprietà per specificare l'alias per la scheda dei giocatori da visualizzare.
3. Eseguire la pagina nel browser. La pagina Visualizza la carta del giocatore di Xbox specificato.

    ![Figura 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
