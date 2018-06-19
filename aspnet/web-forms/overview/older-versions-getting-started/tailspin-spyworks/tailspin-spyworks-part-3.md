---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e i Menu categoria | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882088"
---
<a name="part-3-layout-and-category-menu"></a>Parte 3: Layout e i Menu categoria
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.


## <a id="_Toc260221669"></a>  Aggiunta di un Layout e un Menu di categoria

In questa pagina master sito verrà aggiunto un elemento div per la colonna di sinistra che conterrà il menu di categoria di prodotto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Si noti che l'allineamento desiderato e altri elementi di formattazione vengano fornite dalla classe CSS che abbiamo aggiunto al file Style.css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Il menu di categoria di prodotto verrà creato in modo dinamico in fase di esecuzione da una query sul database di Commerce per i collegamenti esistenti categorie di prodotti e la creazione di voci di menu e corrispondente.

A tale scopo si utilizzeranno due di ASP. Controlli di dati potenti della rete. Il controllo di "Origine dati di entità" e il controllo "ListView di".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Si passa a "Visualizzazione di progettazione" e utilizzare gli helper per configurare i controlli.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Consente di impostare la proprietà di EntityDataSource ID su ripartizione\_categoria\_Menu e fare clic su "Configura origine dati".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Selezionare la connessione CommerceEntities creato automaticamente quando abbiamo creato il modello di origine dati di entità per il Database Commerce e fare clic su "Avanti".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Selezionare il nome del set di entità "Categorie" e lasciare il resto delle opzioni come predefinito. Fare clic su "Fine".

A questo punto è necessario impostare la proprietà ID dell'istanza del controllo ListView che è stato inserito nella nostra pagina ListView\_ProductsMenu e attivare il supporto.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Tuttavia è possibile usare le opzioni di controllo per la formattazione della visualizzazione dell'elemento dati e formattazione, la creazione di menu sarà necessari solo il markup semplice verrà immettere il codice nella visualizzazione origine.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Si noti l'istruzione "Eval": &lt;% # % Eval("CategoryName")&gt;

La sintassi ASP.NET &lt;% # %&gt; è una convenzione a sintassi abbreviata che indica al runtime di eseguire il contenuto è contenuto all'interno e i risultati "nella riga".

L'istruzione Eval("CategoryName") indica che, per la voce corrente nella raccolta associata di elementi di dati, recuperare il valore dei nomi di elemento di modello di entità "CatagoryName". Questa è una sintassi concisa per una funzionalità molto potente.

Consente di eseguire l'applicazione adesso.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Si noti che verrà visualizzato il menu di categoria di prodotto e quando si posiziona su una delle voci di menu categoria, è possibile osservare i punti di collegamento elemento di menu a una pagina è ancora necessario implementare denominato ProductsList.aspx e che è stato compilato un argomento di stringa di query dinamica che contiene il  id della categoria.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-2.md)
> [Successivo](tailspin-spyworks-part-4.md)
