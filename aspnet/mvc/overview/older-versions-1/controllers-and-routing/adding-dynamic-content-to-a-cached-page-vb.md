---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina. Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio banner gli annunci o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: a1747733b190048cac1cb9695dbc1ac24570ee42
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375351"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Aggiunta di contenuto dinamico a una pagina memorizzata nella cache (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come combinare il contenuto dinamico e memorizzati nella cache nella stessa pagina. Sostituzione post-cache consente di visualizzare il contenuto dinamico, ad esempio pubblicitari o notizie, all'interno di una pagina in cui è stata di output memorizzate nella cache.


Sfruttando i vantaggi della memorizzazione nella cache di output, è possibile migliorare notevolmente le prestazioni di un'applicazione ASP.NET MVC. Anziché la rigenerazione di una pagina ogni volta che viene richiesta la pagina, la pagina può essere generata una volta e memorizzata nella cache per più utenti.

Ma si è verificato un problema. Cosa accade se è necessario visualizzare il contenuto dinamico nella pagina? Si supponga, ad esempio, che si desidera visualizzare un banner pubblicitario nella pagina. Non si desidera il banner pubblicitario da memorizzare nella cache in modo che ogni utente veda l'advertisement le stesse. È non risulterebbero alcuna spesa in questo modo.

Fortunatamente, è una pratica soluzione. È possibile sfruttare i vantaggi di una funzionalità del framework ASP.NET chiamato *sostituzione post-cache*. Sostituzione post-cache consente di sostituire il contenuto dinamico in una pagina in cui è stato memorizzato in memoria.


In genere, quando si esegue l'output nella cache una pagina tramite il &lt;OutputCache&gt; attributo, la pagina viene memorizzato nella cache il server sia il client (browser web). Quando si usa la sostituzione post-cache, viene memorizzato nella cache una pagina solo sul server.


#### <a name="using-post-cache-substitution"></a>Usare la sostituzione post-Cache

L'utilizzo di sostituzione post-cache richiede due passaggi. In primo luogo, è necessario definire un metodo che restituisce una stringa che rappresenta il contenuto dinamico che si desidera visualizzare nella pagina memorizzata nella cache. Successivamente, chiamare il metodo HttpResponse.WriteSubstitution() per inserire il contenuto dinamico nella pagina.

Si supponga, ad esempio, che si desidera visualizzare in modo casuale diversi notizie in una pagina memorizzata nella cache. La classe nel listato 1 espone un solo metodo, denominato RenderNews(), che restituisce in modo casuale un elemento di notizie da un elenco di tre articoli di notizie.

**Listato 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Per sfruttare i vantaggi di sostituzione post-cache, chiamare il metodo HttpResponse.WriteSubstitution(). Il metodo WriteSubstitution() imposta il codice per sostituire un'area della pagina memorizzata nella cache con contenuto dinamico. Il metodo WriteSubstitution() consente di visualizzare la notizia casuali nella visualizzazione nel listato 2.

**Listato 2 – Views\Home\Index.aspx.**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

Il metodo RenderNews viene passato al metodo WriteSubstitution(). Si noti che non viene chiamato il metodo RenderNews. Invece viene passato un riferimento al metodo a WriteSubstitution() con l'aiuto dell'operatore AddressOf.

La visualizzazione dell'indice viene memorizzato nella cache. La vista viene restituita dal controller nel listato 3. Si noti che l'azione Index () è decorata con un &lt;OutputCache&gt; attributo che comporti la visualizzazione dell'indice da memorizzare nella cache per 60 secondi.

**Listato 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Anche se la visualizzazione dell'indice viene memorizzato nella cache, gli elementi diversi notizie casuali vengono visualizzati quando si richiede la pagina di indice. Quando si richiede la pagina di indice, l'ora visualizzata dalla pagina rimane invariato per 60 secondi (vedere la figura 1). Il fatto che il cambiamento di ora non è noto che la pagina viene memorizzato nella cache. Tuttavia, il contenuto inserito dal metodo – casuale notizia – WriteSubstitution() viene modificato con ogni richiesta.

**Figura 1 – inserimento dinamici nuovi elementi in una pagina memorizzata nella cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Usare la sostituzione post-Cache nei metodi Helper

Un modo più semplice per sfruttare i vantaggi di sostituzione post-cache è per incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo helper personalizzati. Questo approccio è illustrato il metodo helper nel listato 4.

**Listato 4 – Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

Listato 4 contiene un modulo Visual Basic che espone due metodi: RenderBanner() e RenderBannerInternal(). Il metodo RenderBanner() rappresenta il metodo di supporto effettivo. Questo metodo estende la classe HtmlHelper MVC ASP.NET standard, in modo che sia possibile chiamare Html.RenderBanner() in una vista come qualsiasi altro metodo helper.

Il metodo RenderBanner() chiama il metodo HttpResponse.WriteSubstitution() passando il metodo RenderBannerInternal() al metodo WriteSubsitution().

Il metodo RenderBannerInternal() è un metodo privato. Questo metodo non verrà esposto come un metodo helper. Il metodo RenderBannerInternal() restituisce in modo casuale un'immagine del banner degli annunci da un elenco di tre immagini annuncio banner.

La visualizzazione dell'indice modificata nel listato 5 viene illustrato come è possibile usare il metodo helper RenderBanner(). Si noti che un'ulteriore &lt;% @ % importazione&gt; direttiva è inclusa nella parte superiore della vista per importare lo spazio dei nomi MvcApplication1.Helpers. Se non si importa questo spazio dei nomi, il metodo RenderBanner() non sarà più visualizzato come un metodo sulla proprietà Html.

**Listato 5 – Views\Home\Index.aspx. (con RenderBanner() metodo)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Quando si richiede la pagina sottoposta a rendering da parte della vista nel listato 5, viene visualizzato un annuncio diversi banner con ogni richiesta (vedere la figura 2). La pagina viene memorizzato nella cache, ma l'advertisement banner viene inserito in modo dinamico per il metodo helper RenderBanner().

**Figura 2: la visualizzazione dell'indice la visualizzazione di un annuncio di banner casuale**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione viene illustrato come è possibile aggiornare dinamicamente il contenuto in una pagina memorizzata nella cache. Si è appreso come usare il metodo HttpResponse.WriteSubstitution() per attivare il contenuto dinamico venga inserito in una pagina memorizzata nella cache. Anche appreso come incapsulare la chiamata al metodo WriteSubstitution() all'interno di un metodo di helper HTML.

Sfruttare i vantaggi della memorizzazione nella cache laddove possibile, può avere un impatto significativo sulle prestazioni delle applicazioni web. Come illustrato in questa esercitazione, è possibile sfruttare i vantaggi della memorizzazione nella cache anche quando è necessario visualizzare il contenuto dinamico all'interno delle pagine.

> [!div class="step-by-step"]
> [Precedente](improving-performance-with-output-caching-vb.md)
> [Successivo](creating-a-controller-vb.md)
