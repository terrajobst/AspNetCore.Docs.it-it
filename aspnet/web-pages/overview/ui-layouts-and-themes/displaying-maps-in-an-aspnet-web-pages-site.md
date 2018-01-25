---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualizzazione di mappe in un Web ASP.NET pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene illustrato come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) basato sulla corrispondenza tra servizi forniti da Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 6f3e6a0cfb8c08cd971e88986d0f059dd8237aab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Visualizzazione di mappe in un sito Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) basato sulla corrispondenza tra servizi forniti da Bing, Google, ricerca mappa e Yahoo.
> 
> Illustra quanto segue:
> 
> - Come generare una mappa in base all'indirizzo.
> - Come generare una mappa in base alle coordinate di latitudine e longitudine.
> - Come registrare un Account sviluppatore di Bing Maps e ottenere una chiave da usare con Bing Maps.
> 
> Si tratta della funzionalità ASP.NET introdotta nell'articolo:
> 
> - Il `Maps` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> In questa esercitazione si integra inoltre con 3 di WebMatrix.


Nelle pagine Web, è possibile visualizzare le mappe in una pagina utilizzando `Maps` helper. È possibile generare mappe in base a un indirizzo o su un set di coordinate di longitudine e latitudine. La `Maps` classe consente di chiamare in moduli di gestione di mappa più diffusi tra Bing, Google, ricerca mappa e Yahoo.

I passaggi per aggiungere il mapping a una pagina sono gli stessi indipendentemente da quale dei motori di mappa è chiamare il metodo. È sufficiente aggiungere un riferimento al file JavaScript che rende i metodi disponibili per visualizzare la mappa e quindi si chiamano metodi del `Maps` helper.

Si sceglie un servizio di mappa in base al quale `Maps` metodo helper utilizzato. È possibile utilizzare una di queste:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Installare le parti che necessarie

Per visualizzare le mappe, è necessario queste parti:

- Il `Maps` helper. Questo supporto è in versione 2 di ASP.NET Web Helpers Library. Se non si già aggiunti la libreria, è possibile installare nel sito come pacchetto NuGet. Per informazioni dettagliate, vedere [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372). (Nella raccolta, cercare il `microsoft-web-helpers` pacchetto.)
- La libreria jQuery. Molti dei modelli di sito WebMatrix include già le librerie jQuery in loro *Script* cartelle. Se non si dispone di queste librerie, è possibile scaricare la libreria jQuery più recente direttamente il [jQuery.org](http://jQuery.org) sito. Oppure è possibile creare un nuovo sito usando un modello (ad esempio, il **Starter Site** modello) e quindi copiare i file di jQuery da tale sito al sito corrente.

Infine, se si desidera usare le mappe di Bing, è necessario innanzitutto creare un account (gratuito) e ottenere una chiave. Per ottenere una chiave, seguire questi passaggi:

1. Creare un account sul [Account sviluppatore di Bing mappe](https://www.microsoft.com/maps/developers/web.aspx). È necessario disporre di un account Microsoft (Windows Live ID) nonché.

    È possibile specificare che si desidera utilizzare la chiave per **valutazione/Test**. Se si sta testando la funzione di mapping nel proprio computer mediante WebMatrix e IIS Express, visitare il **sito** dell'area di lavoro e annotare l'URL del sito (ad esempio, `http://localhost:50408`, anche se il numero di porta sarà probabilmente diverso). È possibile utilizzare questo *localhost* indirizzo del sito quando si registra.
2. Dopo avere registrato per un account, il centro Account di Bing Maps, fare clic su **le chiavi Create o visualizzazione**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Prendere nota della chiave che crea Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Creazione di una mappa in base all'indirizzo (tramite Google)

Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base all'indirizzo. In questo esempio viene illustrato come usare le mappe di Google.

1. Creare un file denominato *MapAddress.cshtml* nella radice del sito. Questa pagina genererà una mappa in base all'indirizzo passato a esso.
2. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Tenere presente le seguenti funzionalità della pagina:

    - Il `<script>` elemento il `<head>` elemento. Nell'esempio di `<script>` riferimenti a elementi di *jquery 1.6.4.min.js* file, che è una versione minimizzata (compressa) della libreria jQuery, versione 1.6.4. Si noti che il riferimento si presuppone che il *. js* file è il *script* cartella del sito. 

        > [!NOTE]
        > Se si utilizza una versione diversa della libreria jQuery, assicurarsi che sta correttamente puntano a tale versione.
    - La chiamata al `@Maps.GetGoogleHtml` nel corpo della pagina. Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo. I metodi per i motori di mappa funzionano in modo analogo (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
- Eseguire la pagina e immettere un indirizzo. La pagina Visualizza una mappa, in base alle mappe di Google, che mostra il percorso specificato.

    ![mapping di-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Creazione di una mappa in base a latitudine e longitudine coordinate (tramite Bing)

In questo esempio viene illustrato come creare una mappa in base alle coordinate. In questo esempio viene illustrato come utilizzare Bing maps e su come includere la chiave di Bing. (È possibile creare una mappa in base alle coordinate utilizzando altri motori di mappa, inoltre, senza utilizzare una chiave di Bing).

1. Creare un file denominato *MapCoordinates.cshtml* nella radice del sito e sostituire il contenuto esistente con il codice e markup seguenti:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Sostituire `your-key-here` con la chiave di Bing Maps generato in precedenza.
3. Eseguire il *MapCoordinates.cshtml* immettere coordinate di latitudine e longitudine e quindi fare clic su di **Map It!** immagini (...). (Se non si conoscono tutte le coordinate, procedere come segue. Questo è un percorso nel campus Microsoft Redmond.)

    - Latitude: 47.6781005859375
    - Longitudine:-122.158317565918

    Verrà visualizzata la pagina in base alle coordinate specificate.

    ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Riferimento all'API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
