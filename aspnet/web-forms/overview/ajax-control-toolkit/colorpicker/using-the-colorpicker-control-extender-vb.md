---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Utilizzando l'estensione di controllo ColorPicker (VB) | Documenti Microsoft
author: microsoft
description: ColorPicker è un'estensione di ASP.NET AJAX che fornisce funzionalità di selezione colore sul lato client con l'interfaccia utente in un controllo popup. Può essere collegato a qualsiasi ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-vb"></a>Utilizzando l'estensione di controllo ColorPicker (VB)
====================
by [Microsoft](https://github.com/microsoft)

> ColorPicker è un'estensione di ASP.NET AJAX che fornisce funzionalità di selezione colore sul lato client con l'interfaccia utente in un controllo popup. Può essere collegato a qualsiasi controllo TextBox di ASP.NET. It.


L'obiettivo di questa esercitazione è spiegare come usare il controllo extender AJAX controllo Toolkit ColorPicker. Il controllo extender ColorPicker Visualizza una finestra di dialogo popup che consente di selezionare un colore. Il componente ColorPicker è utile ogni volta che si desidera fornire un'interfaccia utente intuitiva per un utente può selezionare un colore.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estensione di un controllo casella di testo con estensione del controllo ColorPicker

Si supponga, ad esempio, che si desidera creare un sito Web che consente di creare biglietti da visita personalizzati visitatori. I visitatori possono immettere il testo di un biglietto da visita e selezionare il colore. La pagina ASP.NET nel listato 1 contiene due controlli casella di testo denominato txtCardText e txtCardColor. Quando si invia il form, vengono visualizzati i valori selezionati (vedere la figura 1).


[![Forma semplice per la creazione di una scheda di business](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: modulo semplice per la creazione di un biglietto da visita ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image2.png))


**Listing 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Il modulo in works listato 1, ma non fornisce un'esperienza utente ottimale. L'utente dispone di un colore del tipo nella casella di testo. Se l'utente richiede un colore specializzato - ad esempio, solo la destra sfumatura di verde pea - quindi l'utente deve individuare il codice di colore HTML senza l'aiuto.

È possibile utilizzare il controllo extender ColorPicker per creare una migliore esperienza utente. ColorPicker Visualizza una finestra di dialogo colore quando si sposta lo stato attivo a un controllo casella di testo (vedere la figura 2).


[![Estensione del controllo ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: l'estensione di controllo ColorPicker ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image4.png))


È necessario completare due passaggi per utilizzare l'estensione di controllo ColorPicker con il modulo nel listato 1:

1. Aggiungere un controllo ScriptManager alla pagina
2. Aggiungere il controllo extender ColorPicker alla pagina

Prima di poter utilizzare il componente ColorPicker, è necessario aggiungere uno ScriptManager nella pagina. È un ottimo strumento per aggiungere lo ScriptManager sotto l'apertura sul lato server &lt;modulo&gt; tag. È possibile trascinare lo ScriptManager nella pagina dalla casella degli strumenti (ScriptManager si trova nella scheda Estensioni AJAX). In alternativa, è possibile digitare il seguente tag nella visualizzazione origine sotto il tag di apertura modulo sul lato server:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

Il modo più semplice per aggiungere il controllo extender ColorPicker alla pagina è in visualizzazione progettazione. Se si posiziona il puntatore del mouse sulla txtCardColor casella di testo, un'opzione automatica verrà visualizzata la consente di aggiungere un'estensione (vedere la figura 3). Se si seleziona questa opzione, viene visualizzata la procedura guidata Extender (vedere la figura 4).


[![Aggiunta di un'estensione](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: aggiunta di un'estensione ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Selezione di un controllo extender con Creazione guidata](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: la selezione di un controllo extender con la procedura guidata Extender ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image8.png))


È possibile selezionare il programma di estensione per estendere la casella di testo txtCardColor con l'estensione ColorPicker ColorPicker. Fare clic su OK per chiudere la finestra di dialogo.

Dopo aver apportato queste modifiche, l'origine per la pagina è simile a listato 2.

**Elenco di 2 - CreateCard.aspx (con ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Si noti che la pagina contiene ora un controllo ColorPickerExtender visualizzato direttamente sotto il controllo TextBox txtCardColor. Il controllo ColorPickerExtender estende il controllo di txtCardColor in modo che venga visualizzato una finestra di dialogo di selezione colore.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usare un pulsante per avviare la finestra di dialogo di selezione colore

L'estensione ColorPicker supporta le proprietà seguenti:

- PopupButtonId - l'ID di un pulsante alla pagina che fa sì che la finestra di dialogo Selezione colori da visualizzare.
- PopupPosition - la posizione relativa al controllo di destinazione, della finestra di dialogo di selezione colore. I valori possibili sono assoluto, Center, BottomLeft BottomRight, TopLeft, TopRight, a destra e sinistra (il valore predefinito è BottomLeft).
- SampleControlId - l'ID di un controllo che visualizza il colore selezionato.
- SelectedColor - iniziale colore selezionato dal ColorPicker.

È possibile utilizzare queste proprietà per personalizzare la modalità di visualizzazione finestra di dialogo di selezione colore e la modalità di visualizzazione del colore selezionato. La pagina nel listato 3 viene illustrato come utilizzare alcune di queste proprietà.

**Listing 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La pagina nel listato 3 include un colore selezionare pulsante (vedere Figura 5). Quando si fa clic su questo pulsante, viene visualizzata la finestra di dialogo di selezione colore sopra la casella di testo. Se si seleziona un colore dalla finestra di dialogo colore selezionato viene visualizzato come il colore di sfondo di lblSample controllo etichetta.

La proprietà ColorPicker PopupButtonID viene utilizzata per associare il pulsante Seleziona colore con l'estensione ColorPicker. Quando si fornisce un valore per la proprietà PopupButtonID, la finestra di dialogo di selezione colore non apparirà più quando il controllo di destinazione ha lo stato attivo. È necessario fare clic sul pulsante per visualizzare la finestra di dialogo.

La proprietà SampleControlID viene utilizzata per associare un controllo che visualizza il colore selezionato con il componente ColorPicker. Il componente ColorPicker consente di modificare il colore di sfondo del controllo attualmente selezionato.


[![Visualizzare la finestra di selezione colore con un pulsante](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: visualizzare la finestra di selezione colore con un pulsante ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come utilizzare il controllo extender ColorPicker per visualizzare una finestra di dialogo di selezione dei colori popup. Sono state esaminate prima di tutto, viene illustrato come visualizzare la finestra di dialogo quando lo stato attivo viene spostato in un controllo casella di testo. Successivamente, è stato descritto come creare un pulsante che consente di visualizzare la finestra di dialogo di selezione colore quando si fa clic sul pulsante.

> [!div class="step-by-step"]
> [Precedente](using-the-colorpicker-control-extender-cs.md)
