---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Aggiornamenti finali allo spostamento e struttura del sito, conclusioni | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 10 copre aggiornamenti finali allo spostamento e S....
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f32509701dd112053aa4f31d6552601f961c7413
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833328"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: Aggiornamenti finali allo spostamento e struttura del sito, conclusioni
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 10 copre aggiornamenti finali allo spostamento e struttura del sito, conclusioni.


Abbiamo completato tutte le funzionalità principali per il sito, ma sono ancora disponibili alcune funzionalità da aggiungere al riquadro di spostamento del sito, la home page e la pagina Sfoglia Store.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Creazione della visualizzazione parziale riepilogo carrello acquisti

Si vuole esporre il numero di elementi nel carrello acquisti dell'utente nell'intero sito.

![](mvc-music-store-part-10/_static/image1.png)

È possibile implementare facilmente questo mediante la creazione di una visualizzazione parziale che viene aggiunto al nostro Site. master.

Come illustrato in precedenza, il controller ShoppingCart include un metodo di azione CartSummary che restituisce una visualizzazione parziale:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Per creare la visualizzazione parziale CartSummary, pulsante destro del mouse sulla cartella visualizzazioni/ShoppingCart e selezionare Aggiungi visualizzazione. Nome della vista CartSummary e selezionare la casella di controllo "Crea una visualizzazione parziale" come illustrato di seguito.

![](mvc-music-store-part-10/_static/image2.png)

La visualizzazione parziale CartSummary è davvero semplice: è solo un collegamento alla vista ShoppingCart Index che mostra il numero di elementi nel carrello. Il codice completo per CartSummary.cshtml è come segue:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

È possibile includere una visualizzazione parziale in qualsiasi pagina nel sito, incluso il database master del sito, usando il metodo Html.RenderAction. RenderAction richiede di specificare il nome dell'azione ("CartSummary") e il nome del Controller ("ShoppingCart") come di seguito.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Prima di aggiungere questo sito di Layout, verranno create anche nel Menu Genre quindi faciliteremo tutti gli aggiornamenti Site. master in una sola volta.

## <a name="creating-the-genre-menu-partial-view"></a>Creazione della visualizzazione parziale Menu al genere

È possibile rendere molto più semplice per gli utenti per spostarsi tra l'archivio mediante l'aggiunta di un Menu di genere che elenca tutti i generi disponibili nel nostro store.

![](mvc-music-store-part-10/_static/image3.png)

Si seguirà la stessa procedura crea anche una visualizzazione parziale GenreMenu e quindi è possibile aggiungere entrambi al master del sito. In primo luogo, aggiungere l'azione del controller seguente GenreMenu il StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Questa azione restituisce un elenco dei generi che verrà visualizzato tramite la visualizzazione parziale, che verrà creato successivamente.

*Nota: È stato aggiunto l'attributo [ChildActionOnly] all'azione del controller, che indica che si desidera solo questa azione per essere usata da una visualizzazione parziale. Questo attributo eviterà l'azione del controller da in esecuzione passando a /Store/GenreMenu. Non è obbligatorio per le visualizzazioni parziali, ma è buona norma, poiché si vogliono verificare che le azioni del controller vengono usate come Microsoft prevede di rendere. È inoltre sta restituendo PartialView invece di visualizzazione, che consente il motore di visualizzazione di sapere che non devono usare il Layout per questa visualizzazione, come verrà incluso nelle altre visualizzazioni.*

Fare clic su azione del controller GenreMenu e creare una visualizzazione parziale denominata GenreMenu che è fortemente tipizzata utilizzando la classe di dati di visualizzazione Genre, come illustrato di seguito.

![](mvc-music-store-part-10/_static/image4.png)

Aggiornare il codice di visualizzazione per la visualizzazione parziale GenreMenu visualizzare gli elementi usando un elenco non ordinato nel modo seguente.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aggiornamento del Layout del sito per visualizzare viste parziali

È possibile aggiungere viste parziali al Layout del sito (/viste/Shared/\_layout. cshtml) chiamando Html.RenderAction(). Viene aggiunto in entrambi in, nonché alcuni tag aggiuntivi per visualizzarli, come illustrato di seguito:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

A questo punto quando si esegue l'applicazione, si vedrà il genere nell'area di navigazione a sinistra e il riepilogo di carrello nella parte superiore.

## <a name="update-to-the-store-browse-page"></a>Aggiornare la pagina Sfoglia Store

La pagina Sfoglia Store è funzionale, ma non molto bene. È possibile aggiornare la pagina per visualizzare album in un layout ottimale, aggiornare il visualizzazione codice (disponibile in /Views/Store/Browse.cshtml) come indicato di seguito:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Qui seguito all'utilizzo di Action anziché HTML. ActionLink in modo che è possibile applicare una formattazione speciale per il collegamento per includere la grafica di album.

*Nota: Si sta visualizzando una copertina album generico per questi album. Queste informazioni vengono archiviate nel database e può essere modificate tramite la gestione di Store. Si è possibile aggiungere un'immagine personalizzata.*

A questo punto quando si passa a un genere, si vedrà album visualizzato in una griglia con la grafica di album.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aggiornare la Home Page per visualizzare album vendita Top

Vogliamo principali album nella home page per incrementare le vendite di vendita di funzionalità. Apportare alcuni aggiornamenti al nostro HomeController all'handle che e aggiungere anche alcuni grafici aggiuntivi.

In primo luogo, si aggiungerà una proprietà di navigazione per la classe Album in modo che Entity Framework sappia che sono associate. Le ultime righe del nostro **Album** classe dovrebbe ora essere simile al seguente:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Nota: Questo richiederà l'aggiunta di un utilizzo portare nello spazio dei nomi System.Collections.Generic dell'istruzione.*

In primo luogo, si aggiungerà un campo storeDB e il MvcMusicStore.Models usando le istruzioni, come nel nostro altri controller. Successivamente, si aggiungerà il metodo seguente per la classe HomeController che esegue una query dal database per trovare superiore gli album delle vendite in base a OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Si tratta di un metodo privato, poiché non vogliamo per renderlo disponibile come un'azione del controller. Si sta includerlo in HomeController per motivi di semplicità, ma sono invitati a spostare la logica di business in classi di servizio separato come appropriato.

A questo punto, è possibile aggiornare l'azione del controller dell'indice per eseguire query i primi 5 vendere gli album e li tornare alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Il codice completo per la classe HomeController aggiornato è come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Infine, è necessario aggiornare la visualizzazione dell'indice Home in modo che sia possibile visualizzare un elenco di album aggiornando il tipo di modello e aggiungendo l'elenco di album alla fine. Verrà usato l'opportunità di aggiungere anche un'intestazione e una sezione di innalzamento di livello alla pagina.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

A questo punto quando si esegue l'applicazione, si vedrà la home page aggiornata con album di vendita superiore e il nostro messaggio promozionale.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusione

Abbiamo visto che ASP.NET MVC rende più semplice per creare un sito Web sofisticate con accesso al database, l'appartenenza, AJAX, e così via. abbastanza rapidamente. Si spera in questa esercitazione è stata fornita gli strumenti che necessari per iniziare a compilare applicazioni personalizzati ASP.NET MVC.


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-9.md)
