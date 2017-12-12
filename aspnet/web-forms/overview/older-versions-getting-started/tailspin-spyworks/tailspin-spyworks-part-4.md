---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Elenco di prodotti | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 4 viene illustrato l'elenco di prodotti con Contr GridView....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a>Parte 4: Elenco di prodotti
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 4 viene illustrata l'elenco di prodotti con il controllo GridView.


## <a id="_Toc260221670"></a>Elenco di prodotti con il controllo GridView

Esaminiamo implementazione page ProductsList.aspx "Facendo clic" nella soluzione, selezionare "Aggiungi" e "New Item".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Scegliere "Web Form mediante pagina Master" e immettere il nome di una pagina di ProductsList.aspx".

Fare clic su "Aggiungi".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Successivamente, scegliere la cartella "Stili" in cui è stato inserito nella pagina Site. master e selezionarla nella finestra "Il contenuto della cartella".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Fare clic su "Ok" per creare la pagina.

Il database viene popolato con i dati di prodotto come illustrato di seguito.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Dopo aver creata la pagina nuovo verrà utilizzata un'origine dati di entità per accedere ai dati di prodotto, ma in questa istanza è necessario selezionare le entità del prodotto ed è necessario limitare gli elementi che vengono restituiti solo a quelli per la categoria selezionata.

A tale scopo c'EntityDataSource per generare automaticamente la clausola WHERE e si specificano il WhereParameter.

Si ricordi che quando abbiamo creato le voci di Menu nel nostro "Menu categoria prodotto" sono compilate in modo dinamico il collegamento aggiungendo il CatagoryID per il parametro QueryString per ciascun collegamento. Microsoft comunicherà l'origine dati di entità da cui derivare il parametro in cui tale parametro di stringa di query.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Successivamente, è possibile configurare il controllo ListView per visualizzare un elenco di prodotti. Per creare un'esperienza ottimale di acquisto che è sarà compattare diverse funzionalità concisa ogni singolo prodotto visualizzato nel nostro ListVew.

- Il nome del prodotto sarà un collegamento alla visualizzazione dettagli del prodotto.
- Verrà visualizzato il prezzo del prodotto.
- Verrà visualizzata un'immagine del prodotto e si selezionerà in modo dinamico l'immagine da una directory di catalogo immagini nell'applicazione.
- È incluso un collegamento per aggiungere subito il prodotto specifico al carrello.

Di seguito è riportato il markup per l'istanza del controllo ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Stiamo creando dinamicamente diversi collegamenti per ogni prodotto visualizzato.

Inoltre, prima che sia testare una nuova pagina è necessario creare la struttura di directory per il prodotto immagini catalogo come indicato di seguito.

![](tailspin-spyworks-part-4/_static/image1.png)

Una volta le immagini di questo prodotto sono accessibili sia possibile testare la pagina di elenco del prodotto.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Dalla home page del sito, fare clic su uno dei collegamenti elenco categoria.

![](tailspin-spyworks-part-4/_static/image6.jpg)

È necessario implementare la pagina ProductDetials.apsx e la funzionalità AddToCart.

Utilizzare File -&gt;nuovo per creare un nome di pagina ProductDetails.aspx tramite il sito pagina Master, come è stato fatto in precedenza.

Si utilizzerà nuovamente un controllo EntityDataSource per accedere al record di prodotto specifico nel database e si utilizzerà un controllo FormView ASP.NET per visualizzare i dati di prodotto, come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Non occorre preoccuparsi se la formattazione è un po' divertente all'utente. Il markup precedente lascia spazio nel layout di visualizzazione per un paio di funzionalità che verrà implementato in un secondo momento.

Shopping Cart rappresenterà la logica più complessa nell'applicazione. Per iniziare a utilizzare File -&gt;nuovo per creare una pagina denominata MyShoppingCart.aspx.

Si noti che non si sta scelta ShoppingCart.aspx il nome.

Il database contiene una tabella denominata "ShoppingCart". Quando viene generato un Entity Data Model è stata creata una classe per ogni tabella nel database. Pertanto, Entity Data Model generato una classe di entità denominata "ShoppingCart". È possibile modificare il modello in modo che è possibile utilizzare il nome per l'implementazione di carrello acquisti o estenderlo per le esigenze specifiche, ma si è scelto invece a semplicemente selezionare un nome che verrà evitare il conflitto.

È inoltre importante notare che si creeranno un carrello semplice e incorporare la logica di carrello degli acquisti con la visualizzazione del carrello acquisti. È possibile anche scegliere di implementare nostro carrello acquisti in un livello di Business completamente separate.

>[!div class="step-by-step"]
[Precedente](tailspin-spyworks-part-3.md)
[Successivo](tailspin-spyworks-part-5.md)
