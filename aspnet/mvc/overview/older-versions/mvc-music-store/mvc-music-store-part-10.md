---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Aggiornamenti finali allo spostamento e la struttura del sito, conclusione | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 10 copre finali aggiornamenti allo spostamento e S....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: af08039de2d810948b9ab64974111b0346c7fa0f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: Aggiornamenti finali allo spostamento e la struttura del sito, conclusione
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 10 copre finali aggiornamenti allo spostamento e la struttura del sito, conclusione.


Abbiamo completato tutte le funzionalità principali per il sito, ma si dispone ancora di alcune funzionalità da aggiungere per l'esplorazione del sito, la home page e la pagina Sfoglia archivio.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Creazione della visualizzazione parziale riepilogo carrello acquisti

Si vuole esporre il numero di elementi nel carrello acquisti dell'utente in tutto il sito.

![](mvc-music-store-part-10/_static/image1.png)

È possibile implementare facilmente questo mediante la creazione di una visualizzazione parziale che viene aggiunto al nostro Site. master.

Come illustrato in precedenza, il controller ShoppingCart include un metodo di azione CartSummary che restituisce una visualizzazione parziale:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Per creare la visualizzazione parziale CartSummary, fare clic sulla cartella viste/ShoppingCart e selezionare Aggiungi visualizzazione. Nome della vista CartSummary e controllare la casella di controllo "Crea una visualizzazione parziale", come illustrato di seguito.

![](mvc-music-store-part-10/_static/image2.png)

La visualizzazione parziale CartSummary è molto semplice: è solo un collegamento alla visualizzazione che mostra il numero di elementi nel carrello ShoppingCart indice. Il codice completo per CartSummary.cshtml è il seguente:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

È possibile includere una visualizzazione parziale in qualsiasi pagina del sito, inclusi lo schema del sito, tramite il metodo Html.RenderAction. RenderAction richiede di specificare il nome dell'azione ("CartSummary") e il nome del Controller ("ShoppingCart") come di seguito.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Prima di aggiungere questo Layout il sito, si creerà inoltre il genere di Menu per eseguire tutti gli aggiornamenti Site. master in una sola volta.

## <a name="creating-the-genre-menu-partial-view"></a>Creazione della visualizzazione parziale di genere Menu

È possibile rendere molto più semplice per gli utenti per spostarsi tra l'archivio mediante l'aggiunta di un Menu di genere che elenca tutti i generi disponibili nel Negozio.

![](mvc-music-store-part-10/_static/image3.png)

Si seguirà la stessa procedura consente di creare anche una visualizzazione parziale GenreMenu e quindi è possibile aggiungere entrambi allo schema del sito. In primo luogo, aggiungere la seguente azione controller GenreMenu il StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Questa azione restituisce un elenco di generi che verrà visualizzato tramite la visualizzazione parziale, che verrà creato successivamente.

*Nota: È stato aggiunto l'attributo [ChildActionOnly] per questa azione del controller, che indica che si vuole inserire solo questa azione utilizzabile da una visualizzazione parziale. Questo attributo impedirà l'azione del controller venga eseguito selezionando /Store/GenreMenu. Questo non è necessario per le visualizzazioni parziali, ma è consigliabile, poiché è necessario assicurarsi che le azioni del controller vengono usate come si desidera. È inoltre stiamo restituzione PartialView anziché visualizzazione, che informa il motore di visualizzazione che consigliabile utilizzare il Layout per la visualizzazione, come includerlo nelle altre visualizzazioni.*

Fare clic su azione del controller GenreMenu e creare una visualizzazione parziale denominata GenreMenu fortemente tipizzato utilizzando la classe di dati di visualizzazione Genre, come illustrato di seguito.

![](mvc-music-store-part-10/_static/image4.png)

Aggiornare il codice di visualizzazione per la visualizzazione parziale GenreMenu visualizzare gli elementi utilizzando un elenco non ordinato nel modo seguente.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aggiornamento del Layout del sito per visualizzare il nostro visualizzazioni parziali

È possibile aggiungere il nostro visualizzazioni parziali per il Layout del sito (/ / Shared/viste\_cshtml) chiamando Html.RenderAction(). In entrambi, nonché alcuni tag aggiuntivi per visualizzarli, verrà aggiunto come illustrato di seguito:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

A questo punto quando si esegue l'applicazione, verrà spiegato il genere nell'area di navigazione a sinistra e il riepilogo di carrello nella parte superiore.

## <a name="update-to-the-store-browse-page"></a>Aggiornare la pagina Sfoglia archivio

La pagina Sfoglia archivio funzionale, ma non è del tutto soddisfacente. È possibile aggiornare la pagina per visualizzare gli album in un layout ottimale, aggiornare il vista codice (presente in /Views/Store/Browse.cshtml) come indicato di seguito:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Qui stiamo utilizzare Action anziché HTML. ActionLink in modo che è possibile applicare una formattazione speciale per il collegamento per includere il disegno album.

*Nota: Viene visualizzato una copertina generico per gli album. Queste informazioni vengono archiviate nel database e può essere modificato tramite la gestione di Store. Si è di aggiungere un'immagine personalizzata.*

Ora quando si passa a un genere, si visualizzeranno gli album visualizzati in una griglia con la grafica album.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aggiornamento della pagina Home per visualizzare album vendite superiore

Si vuole funzionalità nostri venduti album nella home page per aumentare le vendite. Alcuni aggiornamenti da apportare al nostro HomeController per gestire questa condizione e aggiungere in alcune altre immagini.

In primo luogo, una proprietà di navigazione verrà aggiunto alla classe Album in modo che sia in grado EntityFramework che essi è associati. Le ultime righe del nostro **Album** classe dovrebbe essere simile al seguente:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Nota: Questo richiede l'aggiunta di un utilizzo dell'istruzione di portare nello spazio dei nomi System.Collections.Generic.*

In primo luogo, verrà aggiunto un campo storeDB e il MvcMusicStore.Models utilizzando le istruzioni, come il nostro altri controller. Successivamente, aggiungeremo il metodo seguente per la classe HomeController che esegue query di database per trovare album vendite in base alle OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Si tratta di un metodo privato, poiché non si desidera renderlo disponibile come un'azione del controller. Si incluso in HomeController per motivi di semplicità, ma vengono fornite informazioni utili per spostare la logica di business in classi di servizio separato come appropriato.

A questo punto, è possibile aggiornare l'azione del controller di indice per eseguire query i primi 5 album di vendita e li tornare alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Il codice completo per HomeController aggiornato è come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Infine, è necessario aggiornare la visualizzazione dell'indice Home in modo che è possibile visualizzare un elenco di album l'aggiornamento del tipo di modello e aggiungendo l'elenco di album nella parte inferiore. Si avrà l'opportunità di aggiungere anche un titolo e una sezione di innalzamento di livello alla pagina.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

A questo punto quando si esegue l'applicazione, verranno esaminati aggiornato home page con album vendite e il messaggio promozionale.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusione

Abbiamo visto che tale ASP.NET MVC rende più semplice per creare un sito Web complesse con accesso al database, l'appartenenza, AJAX, e così via. abbastanza rapidamente. Probabilmente questa esercitazione riceve gli strumenti che necessari per iniziare la creazione di applicazioni personalizzate ASP.NET MVC.


>[!div class="step-by-step"]
[Precedente](mvc-music-store-part-9.md)
