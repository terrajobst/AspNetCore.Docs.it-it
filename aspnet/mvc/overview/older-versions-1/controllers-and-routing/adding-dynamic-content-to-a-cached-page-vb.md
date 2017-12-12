---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB) | Documenti Microsoft
author: microsoft
description: Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina. Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio intestazione o di annunci...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina. Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio pubblicitari o notizie, all'interno di una pagina che è stato memorizzato nella cache.


Sfruttando la cache di output, è possibile migliorare notevolmente le prestazioni di un'applicazione MVC ASP.NET. Anziché la rigenerazione di una pagina ogni volta che viene richiesta la pagina, la pagina può essere generata una volta e memorizzata nella cache per più utenti.

Ma si è verificato un problema. Cosa accade se si desidera visualizzare il contenuto dinamico nella pagina? Si supponga, ad esempio, che si desidera visualizzare un banner pubblicitario nella pagina. Non si desidera banner pubblicitario da memorizzare nella cache in modo che ogni utente vede l'annuncio stesso. Si risulterebbero qualsiasi money in questo modo.

Fortunatamente, è una soluzione semplice. È possibile usufruire di una funzionalità del framework di ASP.NET denominato *sostituzione post-cache*. Sostituzione post-cache consente di sostituire il contenuto dinamico in una pagina in cui è stato memorizzato nella cache in memoria.


In genere, quando si esegue l'output nella cache una pagina utilizzando la &lt;OutputCache&gt; attributo, la pagina viene memorizzato nella cache sul server e client (browser web). Quando si utilizza la sostituzione post-cache, una pagina nella cache solo sul server.


#### <a name="using-post-cache-substitution"></a>Utilizzando la sostituzione post-Cache

L'utilizzo di sostituzione post-cache richiede due passaggi. In primo luogo, è necessario definire un metodo che restituisce una stringa che rappresenta il contenuto dinamico che si desidera visualizzare nella pagina memorizzata nella cache. Successivamente, chiamare il metodo HttpResponse.WriteSubstitution() per inserire il contenuto dinamico nella pagina.

Si supponga, ad esempio, che si desidera visualizzare in modo casuale notizie diversi in una pagina memorizzata nella cache. La classe nel listato 1 espone un singolo metodo, denominato RenderNews(), che restituisce in modo casuale una notizia da un elenco di tre elementi di notizie.

**Elenco 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Per sfruttare i vantaggi di sostituzione post-cache, si chiama il metodo HttpResponse.WriteSubstitution(). Il metodo WriteSubstitution() imposta il codice per sostituire un'area della pagina memorizzata nella cache con contenuto dinamico. Il metodo WriteSubstitution() viene utilizzato per visualizzare l'elemento notizie casuali nella visualizzazione elenco 2.

**Elenco di 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

Il metodo RenderNews viene passato al metodo WriteSubstitution(). Si noti che non viene chiamato il metodo RenderNews. Invece viene passato un riferimento al metodo a WriteSubstitution() con l'aiuto dell'operatore AddressOf.

La visualizzazione dell'indice viene memorizzato nella cache. La vista viene restituita dal controller nel listato 3. Si noti che l'azione Index () è decorato con un &lt;OutputCache&gt; attributo che causa la visualizzazione dell'indice da memorizzare nella cache per 60 secondi.

**Elenco di 3: Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Anche se la visualizzazione dell'indice viene memorizzato nella cache, notizie casuali diversi vengono visualizzate quando si richiede la pagina di indice. Quando si richiede la pagina di indice, l'ora visualizzata nella pagina rimane invariato per 60 secondi (vedere la figura 1). Il fatto che il cambiamento di ora non significa che la pagina nella cache. Tuttavia, il contenuto inserito le modifiche di metodo – casuale notizia – WriteSubstitution() con ogni richiesta.

**Figura 1: inserimento di notizie dinamica in una pagina memorizzata nella cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Utilizzo di sostituzione post-Cache in metodi di supporto

Un modo più semplice per sfruttare la sostituzione post-cache è per incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper personalizzati. Questo approccio è illustrato il metodo helper listato 4.

**Listato 4 – Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

Listato 4 contiene un modulo di Visual Basic che espone due metodi: RenderBanner() e RenderBannerInternal(). Il metodo RenderBanner() rappresenta il metodo di supporto effettivo. Questo metodo estende la classe HtmlHelper MVC ASP.NET standard, in modo che sia possibile chiamare Html.RenderBanner() in una vista come qualsiasi altro metodo di supporto.

Il metodo RenderBanner() chiama il metodo HttpResponse.WriteSubstitution() passando il metodo RenderBannerInternal() al metodo WriteSubsitution().

Il metodo RenderBannerInternal() è un metodo privato. Questo metodo non esposto come metodo di supporto. Il metodo RenderBannerInternal() restituisce in modo casuale un'immagine di annuncio dell'intestazione da un elenco di immagini di annuncio tre banner.

La visualizzazione dell'indice modificata nel listato 5 viene illustrato come è possibile utilizzare il metodo di supporto RenderBanner(). Si noti che un altro &lt;% @ Import %&gt; direttiva è inclusa nella parte superiore della vista da importare lo spazio dei nomi MvcApplication1.Helpers. Se non si importa questo spazio dei nomi, il metodo RenderBanner() non verrà visualizzato come un metodo per la proprietà Html.

**Elenco di 5-Views\Home\Index.aspx (con il metodo RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Quando si richiede la pagina il rendering da parte della vista nel listato 5, con ogni richiesta viene visualizzato un annuncio di intestazione diverso (vedere la figura 2). La pagina nella cache, ma il banner pubblicitario viene inserito in modo dinamico dal metodo di supporto RenderBanner().

**Figura 2: la visualizzazione dell'indice la visualizzazione di un annuncio banner casuale**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione viene illustrato come è possibile aggiornare dinamicamente il contenuto in una pagina memorizzata nella cache. È stato descritto come utilizzare il metodo HttpResponse.WriteSubstitution() per attivare il contenuto dinamico essere inseriti in una pagina memorizzata nella cache. È stato inoltre descritto incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper HTML.

Sfruttare i vantaggi della memorizzazione nella cache quando possibile, ciò può avere un impatto significativo sulle prestazioni delle applicazioni web. Come illustrato in questa esercitazione, è possibile sfruttare la memorizzazione nella cache anche quando è necessario visualizzare il contenuto dinamico delle pagine.

>[!div class="step-by-step"]
[Precedente](improving-performance-with-output-caching-vb.md)
[Successivo](creating-a-controller-vb.md)
