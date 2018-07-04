---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Elenco di prodotti | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 4 illustra l'elenco di prodotti con GridView Contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368809"
---
<a name="part-4-listing-products"></a>Parte 4: Elenco di prodotti
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 4 viene illustrato l'elenco di prodotti con il controllo GridView.


## <a id="_Toc260221670"></a>  Elenco di prodotti con il controllo GridView

È possibile iniziare a implementare la pagina ProductsList.aspx facendo clic su"con il pulsante destro" nella nostra soluzione e selezionando "Aggiungi" e "Nuovo elemento".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Scegliere "Web Form mediante pagina Master" e immettere un nome di pagina del ProductsList.aspx".

Fare clic su "Aggiungi".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Quindi scegliere la cartella "Stili" in cui è stato inserito un pagina Site. master e selezionarlo dalla finestra "Contenuto della cartella".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Fare clic su "Ok" per creare la pagina.

Il database viene popolato con dati del prodotto come illustrato di seguito.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Dopo aver creata la pagina si userà anche in questo caso un'origine dati di entità per accedere ai dati del prodotto, ma in questo caso è necessario selezionare le entità Product e dobbiamo limitare gli elementi che vengono restituiti solo a quelli per la categoria selezionata.

A tale scopo possiamo dirti EntityDataSource per generare automaticamente la clausola WHERE e, specifichiamo il WhereParameter.

Si ricordi che quando abbiamo creato le voci di Menu nel nostro "Menu categoria prodotto" viene compilata dinamicamente il collegamento aggiungendo il CatagoryID alla stringa di query per ogni collegamento. Microsoft comunicherà l'origine dati di entità da cui derivare il parametro in cui tale parametro di stringa di query.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Si configurerà quindi il controllo ListView per visualizzare un elenco di prodotti. Per creare un'esperienza di shopping ottimale che è sarà compact diverse funzionalità concisa in ogni singolo prodotto visualizzato nel nostro ListVew.

- Il nome di prodotto è un collegamento alla visualizzazione di dettaglio del prodotto.
- Verrà visualizzato il prezzo del prodotto.
- Verrà visualizzata un'immagine del prodotto e si selezionerà in modo dinamico l'immagine da una directory di immagini di catalogo nella nostra applicazione.
- Verrà incluso un collegamento per aggiungere immediatamente il prodotto specifico al carrello acquisti.

Ecco il markup per l'istanza del controllo ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Stiamo creando dinamicamente diversi collegamenti per ogni prodotto visualizzato.

Inoltre, prima è proprio nuova pagina di test è necessario creare la struttura di directory per il prodotto catalogo immagini come indicato di seguito.

![](tailspin-spyworks-part-4/_static/image1.png)

Dopo che sono accessibili le nostre immagini di prodotto è possibile testare la pagina di elenco del prodotto.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Dalla home page del sito, fare clic su uno dei collegamenti elenco categoria.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Ora è necessario implementare la pagina ProductDetials.apsx e la funzionalità AddToCart.

Utilizzare File -&gt;New per creare un nome di pagina ProductDetails.aspx usando Master Page del sito come in precedenza.

Anche in questo caso si userà un controllo EntityDataSource per accedere al record di prodotto specifico nel database e si userà un controllo FormView ASP.NET per visualizzare i dati del prodotto come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Non occorre preoccuparsi se la formattazione sembra un po' divertente all'utente. Il markup precedente lascia spazio nel layout di visualizzazione per un paio di funzionalità che verrà implementato in un secondo momento.

Il carrello della spesa rappresenterà la logica più complessa nella nostra applicazione. Per iniziare, usare File -&gt;nuovo per creare una pagina denominata MyShoppingCart.aspx.

Si noti che non è stato scelto il nome ShoppingCart.aspx.

Il database contiene una tabella denominata "ShoppingCart". Quando viene generato un Entity Data Model è stata creata una classe per ogni tabella nel database. Pertanto, l'Entity Data Model generato una classe di entità denominato "ShoppingCart". È stato possibile modificare il modello in modo che abbiamo potremmo utilizzare tale nome per l'implementazione di carrello degli acquisti o estenderla per le nostre esigenze, ma si opterà invece a è sufficiente selezionare un nome da evita il conflitto.

È inoltre importante sottolineare che creeremo un carrello acquisti semplice e incorporare la logica di carrello degli acquisti con la visualizzazione del carrello acquisti. Si potrebbe essere anche scegliere di implementare il carrello acquisti in un livello di Business completamente separate.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-3.md)
> [Successivo](tailspin-spyworks-part-5.md)
