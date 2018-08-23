---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Uso dei controlli AJAX controllo Toolkit e controlli Extender (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere i controlli di AJAX Control Toolkit e dispositivi Extender alle pagine ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: f1cf40ec3ba299ee2702258a93aa1192a2112e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823794"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="0f015-103">Uso dei controlli AJAX controllo Toolkit e controlli Extender (VB)</span><span class="sxs-lookup"><span data-stu-id="0f015-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="0f015-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0f015-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0f015-105">Informazioni su come aggiungere i controlli di AJAX Control Toolkit e dispositivi Extender alle pagine ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f015-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="0f015-106">AJAX Control Toolkit contiene un set di controlli e controlli Extender.</span><span class="sxs-lookup"><span data-stu-id="0f015-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="0f015-107">In questa breve esercitazione descrive come aggiungere controlli e controlli Extender per una pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f015-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="0f015-108">Per istruzioni sull'installazione di AJAX Control Toolkit e aggiunta di AJAX Control Toolkit alla casella degli strumenti in Visual Studio e Visual Web Developer, vedere l'esercitazione [Introduzione a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span><span class="sxs-lookup"><span data-stu-id="0f015-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="0f015-109">Utilizzo dei controlli AJAX controllo Toolkit</span><span class="sxs-lookup"><span data-stu-id="0f015-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="0f015-110">Un controllo AJAX Control Toolkit funziona esattamente come un normale controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f015-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="0f015-111">È possibile trascinare il controllo dalla casella degli strumenti in una pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f015-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="0f015-112">È possibile aggiungere il controllo alla pagina in visualizzazione progettazione o visualizzazione di origine.</span><span class="sxs-lookup"><span data-stu-id="0f015-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="0f015-113">È richiesta una speciale quando si usano i controlli di AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0f015-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="0f015-114">La pagina deve contenere un controllo ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="0f015-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="0f015-115">Il controllo ScriptManager è responsabile dell'inclusione di tutto il codice JavaScript necessario richiesto dai controlli AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0f015-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="0f015-116">Ad esempio, la scheda di AJAX Control Toolkit include un controllo denominato il controllo dell'Editor.</span><span class="sxs-lookup"><span data-stu-id="0f015-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="0f015-117">Questo controllo consente di visualizzare un editor HTML completo.</span><span class="sxs-lookup"><span data-stu-id="0f015-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="0f015-118">Seguire questi passaggi per aggiungere il controllo dell'Editor a una pagina:</span><span class="sxs-lookup"><span data-stu-id="0f015-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="0f015-119">Creare una nuova pagina ASP.NET denominata ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="0f015-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="0f015-120">Selezionare il controllo ScriptManager; scheda Estensioni AJAX nella casella degli strumenti e trascinare il controllo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="0f015-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="0f015-121">Selezionare il controllo dell'Editor; la scheda di AJAX Control Toolkit della casella degli strumenti e trascinare il controllo nella pagina (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="0f015-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="0f015-122">La finestra di progettazione dovrebbe essere simile nella figura 2.</span><span class="sxs-lookup"><span data-stu-id="0f015-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="0f015-123">Eseguire il sito web selezionando l'opzione di menu **eseguire il Debug, Avvia debug** o premere F5.</span><span class="sxs-lookup"><span data-stu-id="0f015-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="0f015-124">Verrà visualizzata la pagina nella figura 3.</span><span class="sxs-lookup"><span data-stu-id="0f015-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="0f015-125">[![Selezionare il controllo dell'Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="0f015-126">**Figura 01**: selezionando il controllo HTMLEditor ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="0f015-127">[![Progettazione di Visual Studio con il controllo ScriptManager e modifica](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="0f015-128">**Figura 02**: progettazione di Visual Studio con il controllo ScriptManager e modifica ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="0f015-129">[![La pagina DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="0f015-130">**Figura 03**: pagina DisplayEditor.aspx The ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="0f015-131">Uso di controlli Toolkit controllo Extender di AJAX</span><span class="sxs-lookup"><span data-stu-id="0f015-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="0f015-132">AJAX Control Toolkit contiene anche i dispositivi Extender dei controlli.</span><span class="sxs-lookup"><span data-stu-id="0f015-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="0f015-133">Come suggerisce il nome, un controllo extender estende la funzionalità di un controllo esistente.</span><span class="sxs-lookup"><span data-stu-id="0f015-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="0f015-134">Ad esempio, il controllo extender ConfirmButton estende il controllo pulsante di ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="0f015-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="0f015-135">Il dispositivo extender cambia il comportamento di s controllo pulsante in modo che il pulsante Visualizza una finestra di dialogo di conferma quando si fa clic sopra.</span><span class="sxs-lookup"><span data-stu-id="0f015-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="0f015-136">Un controllo extender, come avviene per un controllo AJAX Control Toolkit, è necessario un controllo ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="0f015-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="0f015-137">È necessario aggiungere un controllo ScriptManager a una pagina prima di iniziare a usare i dispositivi Extender dei controlli nella pagina.</span><span class="sxs-lookup"><span data-stu-id="0f015-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="0f015-138">Seguire questi passaggi per usare il controllo extender ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="0f015-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="0f015-139">Creare una nuova pagina ASP.NET denominata ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="0f015-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="0f015-140">Aggiungere un controllo ScriptManager alla pagina trascinando il controllo alla pagina di livello inferiore a scheda Estensioni AJAX.</span><span class="sxs-lookup"><span data-stu-id="0f015-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="0f015-141">Aggiungere un controllo pulsante alla pagina trascinando il pulsante di livello inferiore della scheda Standard della casella degli strumenti nell'area di progettazione.</span><span class="sxs-lookup"><span data-stu-id="0f015-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="0f015-142">Scegliere il **Aggiungi estensione** opzione attività (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="0f015-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="0f015-143">Nella finestra di dialogo Scegli estensione, selezionare ConfirmButtonExtender (vedere la figura 5) e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="0f015-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="0f015-144">Selezionare il controllo pulsante nella finestra di progettazione ed espandere le estensioni, Button1\_nodo ConfirmButtonExtender nella finestra Proprietà (vedere la figura 6).</span><span class="sxs-lookup"><span data-stu-id="0f015-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="0f015-145">Assegnare il valore *realmente?* alla proprietà ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="0f015-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="0f015-146">Eseguire la pagina selezionando l'opzione di menu **eseguire il Debug, Avvia debug** o premere il tasto F5.</span><span class="sxs-lookup"><span data-stu-id="0f015-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="0f015-147">[![L'opzione dell'attività Aggiungi estensione](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="0f015-148">**Figura 04**: opzione attività l'estensione aggiunta ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="0f015-149">[![Selezionare il controllo extender ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="0f015-150">**Figura 05**: selezionare il controllo extender ConfirmButton ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="0f015-151">[![Impostazione di una proprietà di ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="0f015-152">**Figura 06**: impostare una proprietà di ConfirmButton ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="0f015-153">Quando si apre la pagina, dovrebbe essere un pulsante.</span><span class="sxs-lookup"><span data-stu-id="0f015-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="0f015-154">Quando si fa clic sul pulsante, si ottiene la finestra di dialogo di conferma nella figura 7.</span><span class="sxs-lookup"><span data-stu-id="0f015-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="0f015-155">[![Visualizzare la finestra di dialogo di conferma](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="0f015-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="0f015-156">**Figura 07**: visualizzare la finestra di dialogo di conferma ([fare clic per visualizzare l'immagine con dimensioni normali](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="0f015-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="0f015-157">Si noti che in genere non si trascina un controllo extender in una pagina.</span><span class="sxs-lookup"><span data-stu-id="0f015-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="0f015-158">Usare invece i **Aggiungi estensione** opzione per aggiungere un'estensione a un controllo che già stato aggiunto a una pagina di attività.</span><span class="sxs-lookup"><span data-stu-id="0f015-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="0f015-159">Si noti, inoltre, impostare controllo le proprietà di estensione, aprire la finestra delle proprietà per il controllo esteso.</span><span class="sxs-lookup"><span data-stu-id="0f015-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="0f015-160">Un singolo controllo ASP.NET può essere esteso da più dispositivi Extender dei controlli.</span><span class="sxs-lookup"><span data-stu-id="0f015-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="0f015-161">La finestra delle proprietà per il controllo esteso elencherà tutti i dispositivi Extender dei controlli associati al controllo.</span><span class="sxs-lookup"><span data-stu-id="0f015-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0f015-162">[Precedente](get-started-with-the-ajax-control-toolkit-vb.md)
> [Successivo](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0f015-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
