---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Creazione di un Controller (c#) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther viene illustrato come aggiungere un controller per un'applicazione MVC ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 9faaff1e00998ef9a77c4928a9eb36fc93ab97f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-c"></a>Creazione di un Controller (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther viene illustrato come aggiungere un controller per un'applicazione MVC ASP.NET.


L'obiettivo di questa esercitazione è illustrare come è possibile creare nuovi ASP.NET MVC controller. Informazioni su come creare controller utilizzando l'opzione di menu di Visual Studio aggiungere Controller e creando manualmente un file di classe.

### <a name="using-the-add-controller-menu-option"></a>Tramite l'aggiunta di Controller di menu

Il modo più semplice per creare un nuovo controller consiste nella cartella controller nella finestra Esplora soluzioni di Visual Studio e scegliere il **Aggiungi, Controller** opzione di menu (vedere la figura 1). Selezionando questa opzione di menu viene visualizzata la **Aggiungi Controller** finestra di dialogo (vedere la figura 2).


[![La finestra di dialogo Nuovo progetto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Figura 01**: aggiunge un nuovo controller ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-cs/_static/image2.png))


[![La finestra di dialogo Nuovo progetto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Figura 02**: finestra di dialogo di Aggiungi Controller ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-cs/_static/image4.png))


Si noti che la prima parte del nome del controller è evidenziata nel **Aggiungi Controller** finestra di dialogo. Ogni nome del controller deve terminare con il suffisso *Controller*. Ad esempio, è possibile creare un controller denominato *ProductController* ma non un controller denominato *prodotto*.


Se si crea un controller che manca il *Controller* suffisso quindi non sarà in grado di richiamare il controller. Non eseguire questa operazione, ovvero ho sprecato moltissimo tempo la scadenza dopo questo errore.


**Elenco 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Nella cartella Controllers, è consigliabile creare sempre i controller. In caso contrario, si saranno violano le convenzioni di MVC ASP.NET e altri sviluppatori avrà un'ora più difficile la comprensione dell'applicazione.

### <a name="scaffolding-action-methods"></a>Lo scaffolding di metodi di azione

Quando si crea un controller, è possibile generare automaticamente i metodi di azione Create, Update e dettagli (vedere la figura 3). Se si seleziona questa opzione viene generata la classe di controller nel listato 2.


[![La creazione automatica di metodi di azione](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Figura 03**: la creazione automatica di metodi di azione ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-cs/_static/image6.png))


**Elenco di 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Questi generati sono i metodi stub. È necessario aggiungere la logica effettiva per la creazione, aggiornamento e visualizzazione dei dettagli per un cliente manualmente. Tuttavia, i metodi stub offrono un punto di partenza nice.

### <a name="creating-a-controller-class"></a>Creazione di una classe Controller

Controller MVC ASP.NET è semplicemente una classe. Se si preferisce, è possibile ignorare lo scaffolding di controller pratico di Visual Studio e creare manualmente una classe controller. Attenersi ai passaggi riportati di seguito.

1. Fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, elemento nuovo** e selezionare il **classe** modello (vedere la figura 4).
2. Denominare la nuova classe PersonController.cs e fare clic su di **Aggiungi** pulsante.
3. Modificare il file di classe risultante in modo che la classe eredita dalla classe base MVC (vedere Listato 3).


[![Creazione di una nuova classe](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Figura 04**: creazione di una nuova classe ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-cs/_static/image8.png))


**Elenco di 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Il controller nel listato 3 espone un'azione denominata index () che restituisce la stringa "Hello World!". È possibile richiamare l'azione del controller, che esegue l'applicazione e richiedere un URL simile al seguente:

`http://localhost:40071/Person`

> [!NOTE] 
> 
> Il Server di sviluppo ASP.NET utilizza un numero di porta casuale (ad esempio, 40071). Quando si immette un URL per richiamare un controller, sarà necessario fornire il numero della porta destra. È possibile determinare il numero di porta passando il mouse sull'icona per il Server di sviluppo ASP.NET nell'Area di notifica di Windows (in basso a destra dello schermo).

>[!div class="step-by-step"]
[Precedente](adding-dynamic-content-to-a-cached-page-cs.md)
[Successivo](creating-an-action-cs.md)
