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
<a name="using-the-colorpicker-control-extender-vb"></a><span data-ttu-id="20d05-104">Utilizzando l'estensione di controllo ColorPicker (VB)</span><span class="sxs-lookup"><span data-stu-id="20d05-104">Using the ColorPicker Control Extender (VB)</span></span>
====================
<span data-ttu-id="20d05-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="20d05-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="20d05-106">ColorPicker è un'estensione di ASP.NET AJAX che fornisce funzionalità di selezione colore sul lato client con l'interfaccia utente in un controllo popup.</span><span class="sxs-lookup"><span data-stu-id="20d05-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="20d05-107">Può essere collegato a qualsiasi controllo TextBox di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="20d05-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="20d05-108">It.</span><span class="sxs-lookup"><span data-stu-id="20d05-108">It.</span></span>


<span data-ttu-id="20d05-109">L'obiettivo di questa esercitazione è spiegare come usare il controllo extender AJAX controllo Toolkit ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="20d05-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="20d05-110">Il controllo extender ColorPicker Visualizza una finestra di dialogo popup che consente di selezionare un colore.</span><span class="sxs-lookup"><span data-stu-id="20d05-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="20d05-111">Il componente ColorPicker è utile ogni volta che si desidera fornire un'interfaccia utente intuitiva per un utente può selezionare un colore.</span><span class="sxs-lookup"><span data-stu-id="20d05-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="20d05-112">Estensione di un controllo casella di testo con estensione del controllo ColorPicker</span><span class="sxs-lookup"><span data-stu-id="20d05-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="20d05-113">Si supponga, ad esempio, che si desidera creare un sito Web che consente di creare biglietti da visita personalizzati visitatori.</span><span class="sxs-lookup"><span data-stu-id="20d05-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="20d05-114">I visitatori possono immettere il testo di un biglietto da visita e selezionare il colore.</span><span class="sxs-lookup"><span data-stu-id="20d05-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="20d05-115">La pagina ASP.NET nel listato 1 contiene due controlli casella di testo denominato txtCardText e txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="20d05-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="20d05-116">Quando si invia il form, vengono visualizzati i valori selezionati (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="20d05-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="20d05-117">[![Forma semplice per la creazione di una scheda di business](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20d05-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span></span>

<span data-ttu-id="20d05-118">**Figura 01**: modulo semplice per la creazione di un biglietto da visita ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="20d05-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image2.png))</span></span>


<span data-ttu-id="20d05-119">**Listing 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="20d05-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

<span data-ttu-id="20d05-120">Il modulo in works listato 1, ma non fornisce un'esperienza utente ottimale.</span><span class="sxs-lookup"><span data-stu-id="20d05-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="20d05-121">L'utente dispone di un colore del tipo nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="20d05-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="20d05-122">Se l'utente richiede un colore specializzato - ad esempio, solo la destra sfumatura di verde pea - quindi l'utente deve individuare il codice di colore HTML senza l'aiuto.</span><span class="sxs-lookup"><span data-stu-id="20d05-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="20d05-123">È possibile utilizzare il controllo extender ColorPicker per creare una migliore esperienza utente.</span><span class="sxs-lookup"><span data-stu-id="20d05-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="20d05-124">ColorPicker Visualizza una finestra di dialogo colore quando si sposta lo stato attivo a un controllo casella di testo (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="20d05-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="20d05-125">[![Estensione del controllo ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="20d05-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span></span>

<span data-ttu-id="20d05-126">**Figura 02**: l'estensione di controllo ColorPicker ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="20d05-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image4.png))</span></span>


<span data-ttu-id="20d05-127">È necessario completare due passaggi per utilizzare l'estensione di controllo ColorPicker con il modulo nel listato 1:</span><span class="sxs-lookup"><span data-stu-id="20d05-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="20d05-128">Aggiungere un controllo ScriptManager alla pagina</span><span class="sxs-lookup"><span data-stu-id="20d05-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="20d05-129">Aggiungere il controllo extender ColorPicker alla pagina</span><span class="sxs-lookup"><span data-stu-id="20d05-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="20d05-130">Prima di poter utilizzare il componente ColorPicker, è necessario aggiungere uno ScriptManager nella pagina.</span><span class="sxs-lookup"><span data-stu-id="20d05-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="20d05-131">È un ottimo strumento per aggiungere lo ScriptManager sotto l'apertura sul lato server &lt;modulo&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="20d05-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="20d05-132">È possibile trascinare lo ScriptManager nella pagina dalla casella degli strumenti (ScriptManager si trova nella scheda Estensioni AJAX).</span><span class="sxs-lookup"><span data-stu-id="20d05-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="20d05-133">In alternativa, è possibile digitare il seguente tag nella visualizzazione origine sotto il tag di apertura modulo sul lato server:</span><span class="sxs-lookup"><span data-stu-id="20d05-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="20d05-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="20d05-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="20d05-135">Il modo più semplice per aggiungere il controllo extender ColorPicker alla pagina è in visualizzazione progettazione.</span><span class="sxs-lookup"><span data-stu-id="20d05-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="20d05-136">Se si posiziona il puntatore del mouse sulla txtCardColor casella di testo, un'opzione automatica verrà visualizzata la consente di aggiungere un'estensione (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="20d05-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="20d05-137">Se si seleziona questa opzione, viene visualizzata la procedura guidata Extender (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="20d05-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="20d05-138">[![Aggiunta di un'estensione](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="20d05-138">[![Adding an extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span></span>

<span data-ttu-id="20d05-139">**Figura 03**: aggiunta di un'estensione ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="20d05-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="20d05-140">[![Selezione di un controllo extender con Creazione guidata](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="20d05-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="20d05-141">**Figura 04**: la selezione di un controllo extender con la procedura guidata Extender ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="20d05-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image8.png))</span></span>


<span data-ttu-id="20d05-142">È possibile selezionare il programma di estensione per estendere la casella di testo txtCardColor con l'estensione ColorPicker ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="20d05-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="20d05-143">Fare clic su OK per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="20d05-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="20d05-144">Dopo aver apportato queste modifiche, l'origine per la pagina è simile a listato 2.</span><span class="sxs-lookup"><span data-stu-id="20d05-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="20d05-145">**Elenco di 2 - CreateCard.aspx (con ColorPicker)**</span><span class="sxs-lookup"><span data-stu-id="20d05-145">**Listing 2 - CreateCard.aspx (with ColorPicker)**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

<span data-ttu-id="20d05-146">Si noti che la pagina contiene ora un controllo ColorPickerExtender visualizzato direttamente sotto il controllo TextBox txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="20d05-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="20d05-147">Il controllo ColorPickerExtender estende il controllo di txtCardColor in modo che venga visualizzato una finestra di dialogo di selezione colore.</span><span class="sxs-lookup"><span data-stu-id="20d05-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="20d05-148">Usare un pulsante per avviare la finestra di dialogo di selezione colore</span><span class="sxs-lookup"><span data-stu-id="20d05-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="20d05-149">L'estensione ColorPicker supporta le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="20d05-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="20d05-150">PopupButtonId - l'ID di un pulsante alla pagina che fa sì che la finestra di dialogo Selezione colori da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="20d05-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="20d05-151">PopupPosition - la posizione relativa al controllo di destinazione, della finestra di dialogo di selezione colore.</span><span class="sxs-lookup"><span data-stu-id="20d05-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="20d05-152">I valori possibili sono assoluto, Center, BottomLeft BottomRight, TopLeft, TopRight, a destra e sinistra (il valore predefinito è BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="20d05-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="20d05-153">SampleControlId - l'ID di un controllo che visualizza il colore selezionato.</span><span class="sxs-lookup"><span data-stu-id="20d05-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="20d05-154">SelectedColor - iniziale colore selezionato dal ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="20d05-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="20d05-155">È possibile utilizzare queste proprietà per personalizzare la modalità di visualizzazione finestra di dialogo di selezione colore e la modalità di visualizzazione del colore selezionato.</span><span class="sxs-lookup"><span data-stu-id="20d05-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="20d05-156">La pagina nel listato 3 viene illustrato come utilizzare alcune di queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="20d05-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="20d05-157">**Listing 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="20d05-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

<span data-ttu-id="20d05-158">La pagina nel listato 3 include un colore selezionare pulsante (vedere Figura 5).</span><span class="sxs-lookup"><span data-stu-id="20d05-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="20d05-159">Quando si fa clic su questo pulsante, viene visualizzata la finestra di dialogo di selezione colore sopra la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="20d05-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="20d05-160">Se si seleziona un colore dalla finestra di dialogo colore selezionato viene visualizzato come il colore di sfondo di lblSample controllo etichetta.</span><span class="sxs-lookup"><span data-stu-id="20d05-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="20d05-161">La proprietà ColorPicker PopupButtonID viene utilizzata per associare il pulsante Seleziona colore con l'estensione ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="20d05-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="20d05-162">Quando si fornisce un valore per la proprietà PopupButtonID, la finestra di dialogo di selezione colore non apparirà più quando il controllo di destinazione ha lo stato attivo.</span><span class="sxs-lookup"><span data-stu-id="20d05-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="20d05-163">È necessario fare clic sul pulsante per visualizzare la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="20d05-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="20d05-164">La proprietà SampleControlID viene utilizzata per associare un controllo che visualizza il colore selezionato con il componente ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="20d05-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="20d05-165">Il componente ColorPicker consente di modificare il colore di sfondo del controllo attualmente selezionato.</span><span class="sxs-lookup"><span data-stu-id="20d05-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="20d05-166">[![Visualizzare la finestra di selezione colore con un pulsante](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="20d05-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span></span>

<span data-ttu-id="20d05-167">**Figura 05**: visualizzare la finestra di selezione colore con un pulsante ([fare clic per visualizzare l'immagine ingrandita](using-the-colorpicker-control-extender-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="20d05-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="20d05-168">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="20d05-168">Summary</span></span>

<span data-ttu-id="20d05-169">In questa esercitazione è stato descritto come utilizzare il controllo extender ColorPicker per visualizzare una finestra di dialogo di selezione dei colori popup.</span><span class="sxs-lookup"><span data-stu-id="20d05-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="20d05-170">Sono state esaminate prima di tutto, viene illustrato come visualizzare la finestra di dialogo quando lo stato attivo viene spostato in un controllo casella di testo.</span><span class="sxs-lookup"><span data-stu-id="20d05-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="20d05-171">Successivamente, è stato descritto come creare un pulsante che consente di visualizzare la finestra di dialogo di selezione colore quando si fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="20d05-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="20d05-172">Precedente</span><span class="sxs-lookup"><span data-stu-id="20d05-172">Previous</span></span>](using-the-colorpicker-control-extender-cs.md)
