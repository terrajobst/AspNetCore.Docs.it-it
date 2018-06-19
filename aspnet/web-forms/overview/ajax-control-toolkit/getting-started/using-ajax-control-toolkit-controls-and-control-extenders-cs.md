---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Utilizzo di controlli controllo AJAX e controllo Extender (c#) | Documenti Microsoft
author: microsoft
description: Informazioni su come aggiungere controlli AJAX Control Toolkit ed estensioni per le pagine ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873495"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Utilizzo di controlli controllo AJAX e controllo Extender (c#)
====================
by [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere controlli AJAX Control Toolkit ed estensioni per le pagine ASP.NET.


AJAX Control Toolkit contiene un set di controlli e controllo Extender. In questa breve esercitazione, come aggiungere controlli e controllo Extender per una pagina ASP.NET.

> [!NOTE] 
> 
> Per istruzioni sull'installazione AJAX Control Toolkit e aggiunta di AJAX Control Toolkit nella casella degli strumenti di Visual Studio e Visual Web Developer, vedere l'esercitazione [introduzione AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>Utilizzo di controlli controllo AJAX

Un controllo AJAX Control Toolkit funziona come un normale controllo ASP.NET. È possibile trascinare il controllo dalla casella degli strumenti in una pagina ASP.NET. È possibile aggiungere il controllo alla pagina in visualizzazione progettazione o visualizzazione origine.

È richiesta una speciale quando si utilizzano i controlli di AJAX Control Toolkit. La pagina deve contenere un controllo ScriptManager. Il controllo ScriptManager è responsabile per l'inclusione di tutto il codice JavaScript necessario richiesto dai controlli AJAX Control Toolkit.

Ad esempio, la scheda AJAX Control Toolkit include un controllo denominato il controllo dell'Editor. Questo controllo consente di visualizzare un editor HTML avanzato. Seguire questi passaggi per aggiungere il controllo dell'Editor a una pagina:

1. Creare una nuova pagina ASP.NET denominata ShowEditor.aspx
2. Selezionare il controllo ScriptManager; scheda Estensioni AJAX nella casella degli strumenti e trascinare il controllo nella pagina.
3. Selezionare il controllo dell'Editor; scheda AJAX Control Toolkit nella casella degli strumenti e trascinare il controllo nella pagina (vedere la figura 1). La finestra di progettazione sarà come nella figura 2.
4. Eseguire il sito web selezionando l'opzione di menu **Debug, Avvia debug** o premendo il tasto F5.
5. Verrà visualizzata la pagina nella figura 3.


[![Selezionare il controllo dell'Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figura 01**: selezionare il controllo dell'Editor HTML ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Progettazione di Visual Studio con controllo ScriptManager e modifica](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figura 02**: progettazione di Visual Studio con controllo ScriptManager e modifica ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![La pagina DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figura 03**: pagina di DisplayEditor.aspx ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Utilizzo di AJAX controllo Toolkit controllo Extender

AJAX Control Toolkit contiene inoltre controllo Extender. Come suggerisce il nome, un controllo extender estende la funzionalità di un controllo esistente. Ad esempio, il controllo extender ConfirmButton estende il controllo pulsante di ASP.NET. L'estensione cambia il comportamento di pulsanti di controllo s in modo che il pulsante Visualizza una finestra di dialogo di conferma quando si fa clic sopra.

Un controllo extender, come avviene per un controllo AJAX Control Toolkit, è necessario un controllo ScriptManager. Prima di iniziare a usare le estensioni di controllo nella pagina, è necessario aggiungere un controllo ScriptManager a una pagina.

Seguire questi passaggi per utilizzare l'estensione di controllo ConfirmButton:

1. Creare una nuova pagina ASP.NET denominata ShowConfirmButton.aspx
2. Aggiungere un controllo ScriptManager alla pagina, trascinare il controllo nella pagina di livello inferiore della scheda Estensioni AJAX.
3. Aggiungere un controllo pulsante alla pagina trascinando il pulsante; scheda Standard della casella degli strumenti nell'area di progettazione.
4. Fare clic su di **Aggiungi estensione** attività opzione (vedere la figura 4).
5. Nella finestra di dialogo scegliere Extender selezionare ConfirmButtonExtender (vedere Figura 5) e fare clic sul pulsante OK.
6. Selezionare il controllo pulsante nella finestra di progettazione ed espandere le estensioni, Button1\_nodo ConfirmButtonExtender nella finestra Proprietà (vedere Figura 6). Assegnare il valore *effettivamente?* alla proprietà ConfirmText.
7. Eseguire la pagina selezionando l'opzione di menu **Debug, Avvia debug** o premere il tasto F5.


[![L'opzione di attività Aggiungi estensione](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figura 04**: opzione attività Aggiungi l'estensione ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![Selezionare il controllo extender ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figura 05**: selezionare il controllo extender ConfirmButton ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![Impostazione di una proprietà ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figura 06**: impostare una proprietà ConfirmButton ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Quando si apre la pagina, verrà visualizzato un pulsante. Quando si fa clic sul pulsante, la finestra di dialogo di conferma è visualizzato nella figura 7.


[![Visualizzare la finestra di dialogo di conferma](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figura 07**: visualizzare la finestra di dialogo di conferma ([fare clic per visualizzare l'immagine ingrandita](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Si noti che in genere non si trascina un controllo extender in una pagina. Utilizzare invece il **Aggiungi estensione** opzione per aggiungere un'estensione a un controllo che è già stato aggiunto a una pagina di attività. Si noti, inoltre, impostare controllo delle proprietà di estensione, aprire la finestra delle proprietà per il controllo da estendere.

Un singolo controllo ASP.NET può essere esteso da più estensioni di controllo. Finestra delle proprietà per il controllo viene esteso elencherà tutte le estensioni di controllo associate al controllo.

> [!div class="step-by-step"]
> [Precedente](get-started-with-the-ajax-control-toolkit-cs.md)
> [Successivo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
