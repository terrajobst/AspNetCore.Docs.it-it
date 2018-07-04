---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualizzazione di mappe in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo illustra come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) in base al mapping dei servizi forniti da Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368835"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Visualizzazione di mappe in un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) in base al mapping dei servizi forniti da Bing, Google, MapQuest e Yahoo.
> 
> Che cosa si apprenderà come:
> 
> - Come generare una mappa in base all'indirizzo.
> - Come generare una mappa in base alle coordinate di latitudine e longitudine.
> - Come registrare un Account per sviluppatore di Bing Maps e ottenere una chiave da usare con Bing Maps.
> 
> Si tratta della funzionalità ASP.NET introdotta nell'articolo:
> 
> - Il `Maps` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione si integra inoltre con WebMatrix 3.


Nelle pagine Web, è possibile visualizzare le mappe in una pagina con `Maps` helper. È possibile generare le mappe basate su un indirizzo o su un set di coordinate di longitudine e latitudine. Il `Maps` classe consente di chiamare nei motori di mappa più diffusi tra cui Bing, Google, MapQuest e Yahoo.

I passaggi per l'aggiunta di mapping a una pagina sono gli stessi indipendentemente da quale dei motori di mappa è chiamare. È sufficiente aggiungere un riferimento al file JavaScript che rende disponibili metodi per visualizzare la mappa e quindi si chiamano metodi del `Maps` helper.

Si sceglie un servizio di mappa in base alle quali `Maps` metodo helper utilizzato. È possibile usare uno di questi:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installare le parti che necessarie

Per visualizzare le mappe, sono necessari questi elementi:

- Il `Maps` helper. Questo supporto è nella versione 2 di ASP.NET Web Helpers Library. Se è già stata aggiunta la libreria, è possibile installare nel sito come pacchetto NuGet. Per informazioni dettagliate, vedere [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372). (Nella raccolta, cercare il `microsoft-web-helpers` package.)
- La libreria jQuery. Molti dei modelli di sito WebMatrix include già le librerie jQuery nella loro *Script* cartelle. Se non si dispone di queste librerie, è possibile scaricare la libreria jQuery più recente direttamente dai [jQuery.org](http://jQuery.org) sito. Oppure è possibile creare un nuovo sito usando un modello (ad esempio, il **Starter Site** modello) e quindi copiare i file jQuery da tale sito nel sito corrente.

Infine, se si desidera usare le mappe di Bing, è necessario innanzitutto creare un account (gratuito) e ottenere una chiave. Per ottenere una chiave, seguire questa procedura:

1. Creare un account sul [Account per sviluppatore di Bing mappe](https://www.microsoft.com/maps/developers/web.aspx). È necessario un account Microsoft (Windows Live ID) nonché.

    È possibile specificare che si desidera usare la chiave per **valutazione e Test**. Se si sta testando la funzione di mapping nel proprio computer mediante WebMatrix e IIS Express, visitare il **sito** dell'area di lavoro e annotare l'URL del sito (ad esempio, `http://localhost:50408`, anche se il numero di porta sarà probabilmente diverso). È possibile usare questo *localhost* indirizzo del sito quando si registra.
2. Dopo avere registrato per un account, passare al centro Account mappe di Bing e fare clic su **crea o Visualizza chiavi**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Prendere nota della chiave che crea Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Creazione di una mappa in base all'indirizzo (mediante Google)

Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base all'indirizzo. In questo esempio mostra come usare Google Maps.

1. Creare un file denominato *MapAddress.cshtml* nella radice del sito. Questa pagina genera una mappa in base all'indirizzo che viene passato ad esso.
2. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Si noti che le funzionalità seguenti della pagina:

    - Il `<script>` elemento il `<head>` elemento. Nell'esempio, il `<script>` i riferimenti agli elementi di *jquery 1.6.4.min.js* file, ovvero una versione minimizzata (compressa) della libreria jQuery, versione 1.6.4. Si noti che il riferimento si presuppone che il *js* file si trova nel *script* cartella del sito. 

        > [!NOTE]
        > Se si usa una versione diversa della libreria jQuery, assicurarsi che sta puntando a tale versione correttamente.
    - La chiamata al `@Maps.GetGoogleHtml` nel corpo della pagina. Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo. I metodi per altri motori di mappa funzionano in modo analogo (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Eseguire la pagina e immettere un indirizzo. La pagina viene visualizzata una mappa, basata sulle mappe Google, che mostra la posizione specificata.

     ![1-mapping](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Creazione di una mappa basata su latitudine e longitudine coordina (utilizzando Bing)

In questo esempio viene illustrato come creare una mappa in base alle coordinate. Questo esempio illustra come usare le mappe di Bing e su come includere la chiave di Bing. (È possibile creare una mappa in base alle coordinate con altri motori di mappa anche, senza usare una chiave di Bing).

1. Creare un file denominato *MapCoordinates.cshtml* nella radice del sito e sostituire il contenuto esistente con il codice e il markup seguente:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Sostituire `your-key-here` con la chiave di Bing Maps che è stato generato in precedenza.
3. Eseguire la *MapCoordinates.cshtml* pagina, immettere le coordinate di latitudine e longitudine e quindi fare clic su di **Map It.** immagini (...). (Se non si conoscono le coordinate, procedere come segue. Questo è un percorso all'interno del campus di Redmond Microsoft.)

   - Latitudine: 47.6781005859375
   - Longitudine:-122.158317565918

     Verrà visualizzata la pagina in base alle coordinate specificato.

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Riferimento all'API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
