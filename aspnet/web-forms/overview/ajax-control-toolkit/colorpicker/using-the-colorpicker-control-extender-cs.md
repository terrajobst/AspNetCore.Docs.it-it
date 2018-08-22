---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Tramite il controllo Extender ColorPicker (c#) | Microsoft Docs
author: microsoft
description: ColorPicker è un extender di AJAX ASP.NET che fornisce funzionalità di selezione colore lato client con l'interfaccia utente in un controllo popup. Può essere collegato a qualsiasi ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b8d581ed426227ed77435e22c84e9ea5d62ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826379"
---
<a name="using-the-colorpicker-control-extender-c"></a>Tramite il controllo Extender ColorPicker (c#)
====================
da [Microsoft](https://github.com/microsoft)

> ColorPicker è un extender di AJAX ASP.NET che fornisce funzionalità di selezione colore lato client con l'interfaccia utente in un controllo popup. Può essere collegato a qualsiasi controllo TextBox ASP.NET. Si tratta.


L'obiettivo di questa esercitazione è illustrare come è possibile usare il controllo extender Toolkit ColorPicker di controllo AJAX. Il controllo extender ColorPicker Visualizza una finestra di dialogo popup che consente di selezionare un colore. ColorPicker è utile ogni volta che si desidera fornire un'interfaccia utente intuitiva per un utente di scegliere un colore.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estensione di un controllo casella di testo con il controllo Extender ColorPicker

Si supponga, ad esempio, che si desidera creare un sito Web che consente ai visitatori di creare schede di business personalizzate. I visitatori possono immettere il testo di un biglietto da visita e scegliere il colore. La pagina ASP.NET nel listato 1 contiene due controlli TextBox denominato txtCardText e txtCardColor. Quando si invia il form, i valori selezionati vengono visualizzati (vedere la figura 1).


[![Modulo semplice per la creazione di un biglietto da visita](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Figura 01**: modulo semplice per la creazione di un biglietto da visita ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Listato 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Il modulo in funzionamento del listato 1, ma non fornisce un'esperienza utente eccezionale. L'utente deve digitare un colore nella casella di testo. Se l'utente desidera impostare un colore specializzato -, ad esempio, semplicemente la sfumatura a destra di unta: verde - quindi l'utente deve individuare il codice di colore HTML senza alcun aiuto.

È possibile usare il controllo extender ColorPicker per creare un'esperienza utente migliore. ColorPicker Visualizza una finestra di dialogo colore quando si sposta lo stato attivo a un controllo casella di testo (vedere la figura 2).


[![Il controllo Extender ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Figura 02**: l'oggetto Extender ColorPicker ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-cs/_static/image4.png))


È necessario completare due passaggi per usare il controllo extender ColorPicker con il modulo nel listato 1:

1. Aggiungere un controllo ScriptManager alla pagina
2. Aggiungere il controllo extender ColorPicker alla pagina

Prima di poter usare ColorPicker, è necessario aggiungere uno ScriptManager nella pagina. Un buon punto di aggiunta di ScriptManager è sotto il server-side di apertura &lt;form&gt; tag. È possibile trascinare ScriptManager nella pagina dalla casella degli strumenti (ScriptManager si trova nella scheda Estensioni AJAX). In alternativa, è possibile digitare il seguente tag nella visualizzazione origine sotto il tag di apertura form lato server:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

Il modo più semplice per aggiungere il controllo extender ColorPicker alla pagina è in visualizzazione progettazione. Se si posiziona il puntatore del mouse txtCardColor nella casella di testo, un'opzione automatica verrà visualizzata la consente è quindi necessario aggiungere un'estensione (vedere la figura 3). Se si seleziona questa opzione, viene visualizzata la procedura guidata estensione (vedere la figura 4).


[![Aggiunta di un dispositivo extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Figura 03**: aggiunta di un extender di ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Selezione di un controllo extender con la procedura guidata dispositivo Extender](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Figura 04**: la selezione di un controllo extender con la procedura guidata estensione ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-cs/_static/image8.png))


È possibile selezionare il dispositivo extender ColorPicker per estendere il txtCardColor nella casella di testo con il dispositivo extender ColorPicker. Fare clic su OK per chiudere la finestra di dialogo.

Dopo aver apportato queste modifiche, l'origine per la pagina è simile a listato 2.

Listato 2 - CreateCard.aspx (con ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Si noti che la pagina contiene ora un controllo ColorPickerExtender visualizzata direttamente sotto il controllo TextBox txtCardColor. Il controllo ColorPickerExtender estende il controllo txtCardColor affinché visualizzi una finestra di dialogo di selezione colore.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usare un pulsante per avviare la finestra di dialogo di selezione colore

Il dispositivo extender ColorPicker supporta le proprietà seguenti:

- PopupButtonId - l'ID di un pulsante della pagina che fa sì che la finestra di dialogo Selezione colore da visualizzare.
- PopupPosition - la posizione, relativo al controllo di destinazione, della finestra di dialogo di selezione colore. I valori possibili sono assoluti, centro, BottomLeft, BottomRight, TopLeft, alto, destra e a sinistra (il valore predefinito è BottomLeft).
- SampleControlId - l'ID di un controllo che visualizza il colore selezionato.
- SelectedColor - il colore iniziale per ColorPicker selezionato.

È possibile utilizzare queste proprietà per personalizzare il modo in cui viene visualizzata la finestra di dialogo di selezione di colore e la modalità di visualizzazione del colore selezionato. La pagina nel listato 3 illustra come è possibile utilizzare alcune di queste proprietà.

**Listato 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

La pagina nel listato 3 include un colore fate clic sul pulsante (vedere la figura 5). Quando si fa clic su questo pulsante, viene visualizzata la finestra di dialogo di selezione colore sopra la casella di testo. Se si seleziona un colore dalla finestra di dialogo colore selezionato viene visualizzato come colore di sfondo per il controllo etichetta lblSample.

La proprietà ColorPicker PopupButtonID viene utilizzata per associare il pulsante Seleziona colore con il dispositivo extender ColorPicker. Quando si fornisce un valore per la proprietà PopupButtonID, la finestra di dialogo di selezione colore non compare più quando il controllo di destinazione ha lo stato attivo. È necessario fare clic sul pulsante per visualizzare la finestra di dialogo.

La proprietà SampleControlID viene utilizzata per associare un controllo che visualizza il colore selezionato con ColorPicker. ColorPicker di modificare il colore di sfondo di questo controllo attualmente selezionato.


[![Visualizzare la finestra di dialogo di selezione dei colori con un pulsante](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Figura 05**: visualizzare la finestra di dialogo di selezione dei colori con un pulsante ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come utilizzare il controllo extender ColorPicker per visualizzare una finestra di dialogo popup selezione colore. In primo luogo, abbiamo esaminato come è possibile visualizzare la finestra di dialogo quando lo stato attivo viene spostato in un controllo casella di testo. Successivamente, si è appreso come creare un pulsante che visualizza la finestra di dialogo di selezione colore quando si fa clic sul pulsante.

> [!div class="step-by-step"]
> [avanti](using-the-colorpicker-control-extender-vb.md)
