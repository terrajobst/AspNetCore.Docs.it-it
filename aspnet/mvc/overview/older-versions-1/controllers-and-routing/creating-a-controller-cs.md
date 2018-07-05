---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Creazione di un Controller (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come aggiungere un controller a un'applicazione ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b3b663f76b6271a213e5980756ba63372ef6fe9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804266"
---
<a name="creating-a-controller-c"></a>Creazione di un Controller (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther spiega come aggiungere un controller a un'applicazione ASP.NET MVC.


L'obiettivo di questa esercitazione è illustrare come è possibile creare nuovi ASP.NET MVC controller. Informazioni su come creare controller usando l'opzione di menu di Visual Studio aggiungere Controller e creando manualmente un file di classe.

### <a name="using-the-add-controller-menu-option"></a>Con l'aggiungere Controller di menu

Il modo più semplice per creare un nuovo controller consiste nella cartella Controllers nella finestra Esplora soluzioni di Visual Studio e scegliere il **Controller, Aggiungi** l'opzione di menu (vedere la figura 1). Selezionando questa opzione di menu si apre la **Aggiungi Controller** finestra di dialogo (vedere la figura 2).


[![La finestra di dialogo Nuovo progetto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Figura 01**: aggiunge un nuovo controller ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image2.png))


[![La finestra di dialogo Nuovo progetto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Figura 02**: finestra di dialogo Aggiungi Controller The ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image4.png))


Si noti che la prima parte del nome del controller sia evidenziata nel **Aggiungi Controller** finestra di dialogo. Ogni nome del controller deve terminare con il suffisso *Controller*. Ad esempio, è possibile creare un controller denominato *ProductController* ma non un controller denominato *prodotto*.


Se si crea un controller che non è presente il *Controller* suffisso, quindi sarà possibile richiamare il controller. Non eseguire questa operazione, ho sprecato innumerevoli ore della vita dopo commettere questo errore.


**Listato 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

È sempre necessario creare i controller nella cartella controller. In caso contrario, è possibile violare le convenzioni di ASP.NET MVC e altri sviluppatori avrà un tempo più difficile la comprensione dell'applicazione.

### <a name="scaffolding-action-methods"></a>Lo scaffolding di metodi di azione

Quando si crea un controller, è possibile generare automaticamente i metodi di azione Create, Update e dettagli (vedere la figura 3). Se si seleziona questa opzione viene generata la classe controller nel listato 2.


[![La creazione automatica di metodi di azione](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Figura 03**: la creazione automatica di metodi di azione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image6.png))


**Listato 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Questi metodi generati si trovano i metodi stub. È necessario aggiungere la logica effettiva per la creazione, aggiornamento e che mostra i dettagli per un cliente se stessi. Tuttavia, i metodi stub forniscono un punto di partenza utile.

### <a name="creating-a-controller-class"></a>Creazione di una classe Controller

Il controller ASP.NET MVC è semplicemente una classe. Se si preferisce, è possibile ignorare lo scaffolding di controller pratico di Visual Studio e creare manualmente una classe controller. Attenersi ai passaggi riportati di seguito.

1. Fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, elemento nuove** e selezionare il **classe** modello (vedere la figura 4).
2. Nome nuova classe PersonController.cs, quindi scegliere il **Add** pulsante.
3. Modificare il file di classe risultante in modo che la classe eredita dalla classe di MVC di base (vedere il listato 3).


[![Creazione di una nuova classe](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Figura 04**: la creazione di una nuova classe ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-controller-cs/_static/image8.png))


**Listato 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Il controller nel listato 3 espone un'azione denominata index () che restituisce la stringa "Hello World!". È possibile richiamare l'azione del controller, eseguire l'applicazione e che richiede un URL simile al seguente:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Il Server di sviluppo ASP.NET utilizza un numero di porta casuale (ad esempio, 40071). Quando si immette un URL per richiamare un controller, è necessario fornire il numero della porta destra. È possibile determinare il numero di porta posizionando il puntatore del mouse sull'icona per il Server di sviluppo ASP.NET nell'Area di notifica Windows (in basso a destra dello schermo).
> 
> [!div class="step-by-step"]
> [Precedente](adding-dynamic-content-to-a-cached-page-cs.md)
> [Successivo](creating-an-action-cs.md)
