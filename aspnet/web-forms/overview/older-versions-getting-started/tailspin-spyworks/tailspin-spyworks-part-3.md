---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e Menu per la categoria | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 72642610c203127b431e03214b2f1bc85a85b5fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366647"
---
<a name="part-3-layout-and-category-menu"></a>Parte 3: Layout e Menu per la categoria
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.


## <a id="_Toc260221669"></a>  Aggiunta di alcuni Layout e un Menu di categoria

In questa pagina master del sito verrà aggiunto un elemento div per la colonna di sinistra che conterrà il menu di scelta di categoria di prodotto.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Si noti che l'allineamento desiderato e altre opzioni di formattazione saranno disponibili per la classe CSS che abbiamo aggiunto al file Style. CSS.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Nel menu di categoria di prodotto verrà creato in modo dinamico in fase di esecuzione da una query sul database di e-Commerce per i collegamenti esistenti categorie di prodotti e la creazione di voci di menu e corrispondente.

A tale scopo si userà due di ASP. Controlli avanzati per i dati di NET. Il controllo "Entità origine di dati" e il controllo "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

È possibile passare alla "Visualizzazione di progettazione" e usare gli helper per configurare i controlli.

![](tailspin-spyworks-part-3/_static/image2.jpg)

È possibile impostare la proprietà ID di EntityDataSource per EDS\_categoria\_Menu e fare clic su "Configura origine dati".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Selezionare la connessione CommerceEntities che è stato creato automaticamente quando abbiamo creato il modello di origine dati di entità per il Database di e-Commerce e fare clic su "Avanti".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Selezionare il nome del set di entità "Categorie" e lasciare le altre opzioni come predefinito. Fare clic su "Fine".

A questo punto è possibile impostare la proprietà ID dell'istanza del controllo ListView che è stato inserito nella nostra pagina ListView\_ProductsMenu e attivare il supporto.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Tuttavia è possibile utilizzare le opzioni di controllo per formattare la visualizzazione di elementi di dati e formattazione, la creazione di menu sarà necessari solo markup semplice verrà immettere il codice nella visualizzazione origine.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Si noti l'istruzione "Eval": &lt;% # Eval("CategoryName") %&gt;

La sintassi ASP.NET &lt;% # %&gt; è una convenzione a sintassi abbreviata che indica al runtime di eseguire qualsiasi elemento è contenuto all'interno e restituire i risultati "nella riga".

L'istruzione Eval("CategoryName") indica che, per la voce corrente nella raccolta associata di elementi di dati, recuperare il valore dei nomi di elemento Entity Model "CatagoryName". Si tratta di una sintassi concisa per una funzionalità molto potente.

Consente di eseguire ora l'applicazione.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Si noti che viene ora visualizzato il menu di scelta di categoria di prodotto e quando si al passaggio del mouse su una delle voci di menu categoria noteremo i punti di collegamento elemento menu a una pagina si è ancora necessario implementare denominato ProductsList.aspx e che è stato creato un argomento di stringa di query dinamica che contiene il  id della categoria.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-2.md)
> [Successivo](tailspin-spyworks-part-4.md)
