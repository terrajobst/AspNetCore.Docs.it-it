---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Uso dei controlli AJAX controllo Toolkit e controlli Extender (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere i controlli di AJAX Control Toolkit e dispositivi Extender alle pagine ASP.NET.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 292a6e41740538d41e26b01fa76ff59f9af21221
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825360"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Uso dei controlli AJAX controllo Toolkit e controlli Extender (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere i controlli di AJAX Control Toolkit e dispositivi Extender alle pagine ASP.NET.


AJAX Control Toolkit contiene un set di controlli e controlli Extender. In questa breve esercitazione descrive come aggiungere controlli e controlli Extender per una pagina ASP.NET.

> [!NOTE] 
> 
> Per istruzioni sull'installazione di AJAX Control Toolkit e aggiunta di AJAX Control Toolkit alla casella degli strumenti in Visual Studio e Visual Web Developer, vedere l'esercitazione [Introduzione a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).


## <a name="using-ajax-control-toolkit-controls"></a>Utilizzo dei controlli AJAX controllo Toolkit

Un controllo AJAX Control Toolkit funziona esattamente come un normale controllo ASP.NET. È possibile trascinare il controllo dalla casella degli strumenti in una pagina ASP.NET. È possibile aggiungere il controllo alla pagina in visualizzazione progettazione o visualizzazione di origine.

È richiesta una speciale quando si usano i controlli di AJAX Control Toolkit. La pagina deve contenere un controllo ScriptManager. Il controllo ScriptManager è responsabile dell'inclusione di tutto il codice JavaScript necessario richiesto dai controlli AJAX Control Toolkit.

Ad esempio, la scheda di AJAX Control Toolkit include un controllo denominato il controllo dell'Editor. Questo controllo consente di visualizzare un editor HTML completo. Seguire questi passaggi per aggiungere il controllo dell'Editor a una pagina:

1. Creare una nuova pagina ASP.NET denominata ShowEditor.aspx
2. Selezionare il controllo ScriptManager; scheda Estensioni AJAX nella casella degli strumenti e trascinare il controllo nella pagina.
3. Selezionare il controllo dell'Editor; la scheda di AJAX Control Toolkit della casella degli strumenti e trascinare il controllo nella pagina (vedere la figura 1). La finestra di progettazione dovrebbe essere simile nella figura 2.
4. Eseguire il sito web selezionando l'opzione di menu **eseguire il Debug, Avvia debug** o premere F5.
5. Verrà visualizzata la pagina nella figura 3.


[![Selezionare il controllo dell'Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Figura 01**: selezionando il controllo HTMLEditor ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![Progettazione di Visual Studio con il controllo ScriptManager e modifica](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Figura 02**: progettazione di Visual Studio con il controllo ScriptManager e modifica ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![La pagina DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Figura 03**: pagina DisplayEditor.aspx The ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Uso di controlli Toolkit controllo Extender di AJAX

AJAX Control Toolkit contiene anche i dispositivi Extender dei controlli. Come suggerisce il nome, un controllo extender estende la funzionalità di un controllo esistente. Ad esempio, il controllo extender ConfirmButton estende il controllo pulsante di ASP.NET standard. Il dispositivo extender cambia il comportamento di s controllo pulsante in modo che il pulsante Visualizza una finestra di dialogo di conferma quando si fa clic sopra.

Un controllo extender, come avviene per un controllo AJAX Control Toolkit, è necessario un controllo ScriptManager. È necessario aggiungere un controllo ScriptManager a una pagina prima di iniziare a usare i dispositivi Extender dei controlli nella pagina.

Seguire questi passaggi per usare il controllo extender ConfirmButton:

1. Creare una nuova pagina ASP.NET denominata ShowConfirmButton.aspx
2. Aggiungere un controllo ScriptManager alla pagina trascinando il controllo alla pagina di livello inferiore a scheda Estensioni AJAX.
3. Aggiungere un controllo pulsante alla pagina trascinando il pulsante di livello inferiore della scheda Standard della casella degli strumenti nell'area di progettazione.
4. Scegliere il **Aggiungi estensione** opzione attività (vedere la figura 4).
5. Nella finestra di dialogo Scegli estensione, selezionare ConfirmButtonExtender (vedere la figura 5) e fare clic sul pulsante OK.
6. Selezionare il controllo pulsante nella finestra di progettazione ed espandere le estensioni, Button1\_nodo ConfirmButtonExtender nella finestra Proprietà (vedere la figura 6). Assegnare il valore *realmente?* alla proprietà ConfirmText.
7. Eseguire la pagina selezionando l'opzione di menu **eseguire il Debug, Avvia debug** o premere il tasto F5.


[![L'opzione dell'attività Aggiungi estensione](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Figura 04**: opzione attività l'estensione aggiunta ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![Selezionare il controllo extender ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Figura 05**: selezionare il controllo extender ConfirmButton ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![Impostazione di una proprietà di ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Figura 06**: impostare una proprietà di ConfirmButton ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


Quando si apre la pagina, dovrebbe essere un pulsante. Quando si fa clic sul pulsante, si ottiene la finestra di dialogo di conferma nella figura 7.


[![Visualizzare la finestra di dialogo di conferma](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Figura 07**: visualizzare la finestra di dialogo di conferma ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


Si noti che in genere non si trascina un controllo extender in una pagina. Usare invece i **Aggiungi estensione** opzione per aggiungere un'estensione a un controllo che già stato aggiunto a una pagina di attività. Si noti, inoltre, impostare controllo le proprietà di estensione, aprire la finestra delle proprietà per il controllo esteso.

Un singolo controllo ASP.NET può essere esteso da più dispositivi Extender dei controlli. La finestra delle proprietà per il controllo esteso elencherà tutti i dispositivi Extender dei controlli associati al controllo.

> [!div class="step-by-step"]
> [Precedente](get-started-with-the-ajax-control-toolkit-vb.md)
> [Successivo](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
