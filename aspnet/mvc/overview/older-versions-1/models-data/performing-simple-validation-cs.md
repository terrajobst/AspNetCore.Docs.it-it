---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Esegue una convalida semplice (c#) | Microsoft Docs
author: StephenWalther
description: Informazioni su come eseguire la convalida in un'applicazione ASP.NET MVC. In questa esercitazione, Stephen Walther introduce per lo stato del modello e l'helper di convalida HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 622d4bc40b2f403c0a4efe0ee799a68f1adbe4fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377415"
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="17d8b-104">Esegue una convalida semplice (c#)</span><span class="sxs-lookup"><span data-stu-id="17d8b-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="17d8b-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="17d8b-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="17d8b-106">Informazioni su come eseguire la convalida in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17d8b-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="17d8b-107">In questa esercitazione, Stephen Walther introduce per lo stato del modello e gli helper di convalida HTML.</span><span class="sxs-lookup"><span data-stu-id="17d8b-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="17d8b-108">L'obiettivo di questa esercitazione è illustrare come è possibile eseguire la convalida all'interno di un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17d8b-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="17d8b-109">Ad esempio, informazioni su come impedire a qualcuno di invio di un modulo che non contiene un valore per un campo obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="17d8b-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="17d8b-110">Descrive come usare lo stato del modello e gli helper HTML di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="17d8b-111">Informazioni sullo stato del modello</span><span class="sxs-lookup"><span data-stu-id="17d8b-111">Understanding Model State</span></span>

<span data-ttu-id="17d8b-112">Si usa lo stato del modello - o, più precisamente, dizionario di stato del modello - per rappresentare errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="17d8b-113">L'azione create () nel listato 1, ad esempio, convalida le proprietà di una classe di prodotto prima di aggiungere la classe di prodotto a un database.</span><span class="sxs-lookup"><span data-stu-id="17d8b-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="17d8b-114">Non sto consigliata aggiungere la logica di convalida o di database a un controller.</span><span class="sxs-lookup"><span data-stu-id="17d8b-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="17d8b-115">Un controller deve contenere solo la logica correlata al controllo di flusso dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="17d8b-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="17d8b-116">Si sta eseguendo un collegamento a semplificare le operazioni.</span><span class="sxs-lookup"><span data-stu-id="17d8b-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="17d8b-117">**Listato 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="17d8b-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="17d8b-118">Nel listato 1, vengono convalidate le proprietà di nome, descrizione e UnitsInStock della classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="17d8b-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="17d8b-119">Se una di queste proprietà ha esito negativo un test di convalida viene aggiunto un errore al dizionario di stato del modello (rappresentato dalla proprietà ModelState della classe Controller).</span><span class="sxs-lookup"><span data-stu-id="17d8b-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="17d8b-120">Se sono presenti errori nello stato del modello la proprietà ModelState restituisce false.</span><span class="sxs-lookup"><span data-stu-id="17d8b-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="17d8b-121">In tal caso, viene visualizzata di nuovo il form HTML per la creazione di un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="17d8b-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="17d8b-122">In caso contrario, se non sono presenti errori di convalida, il nuovo prodotto viene aggiunto al database.</span><span class="sxs-lookup"><span data-stu-id="17d8b-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="17d8b-123">Usando gli helper di convalida</span><span class="sxs-lookup"><span data-stu-id="17d8b-123">Using the Validation Helpers</span></span>

<span data-ttu-id="17d8b-124">Il framework ASP.NET MVC include due helper di convalida: l'helper Html.ValidationMessage() e l'helper Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="17d8b-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="17d8b-125">Utilizzare questi due helper in una vista per visualizzare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="17d8b-126">Gli helper Html.ValidationMessage() e Html.ValidationSummary() vengono usati nelle visualizzazioni Creazione e modifica vengono generate automaticamente per lo scaffolding di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17d8b-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="17d8b-127">Seguire questi passaggi per generare la visualizzazione di creazione:</span><span class="sxs-lookup"><span data-stu-id="17d8b-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="17d8b-128">L'azione create () nel controller di prodotto e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="17d8b-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="17d8b-129">Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata** (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="17d8b-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="17d8b-130">Dal **visualizzare i dati classe** elenco a discesa selezionare la classe Product.</span><span class="sxs-lookup"><span data-stu-id="17d8b-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="17d8b-131">Dal **visualizzare il contenuto** dall'elenco a discesa selezionare Crea.</span><span class="sxs-lookup"><span data-stu-id="17d8b-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="17d8b-132">Fare clic sul pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="17d8b-132">Click the **Add** button.</span></span>


<span data-ttu-id="17d8b-133">Assicurarsi che si compila l'applicazione prima di aggiungere una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="17d8b-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="17d8b-134">In caso contrario, non sarà più visualizzato l'elenco delle classi nel **visualizzare i dati classe** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="17d8b-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="17d8b-135">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17d8b-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="17d8b-136">**Figura 01**: aggiunta di una vista ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="17d8b-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="17d8b-137">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="17d8b-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="17d8b-138">**Figura 02**: creazione di una visualizzazione fortemente tipizzata ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="17d8b-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="17d8b-139">Dopo aver completato questi passaggi, si ottiene la visualizzazione di creazione nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="17d8b-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="17d8b-140">**Listato 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="17d8b-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="17d8b-141">Nel listato 2, l'helper Html.ValidationSummary() viene chiamato immediatamente sopra il form HTML.</span><span class="sxs-lookup"><span data-stu-id="17d8b-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="17d8b-142">Questo supporto consente di visualizzare un elenco di messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="17d8b-143">L'helper Html.ValidationSummary() esegue il rendering gli errori in un elenco puntato.</span><span class="sxs-lookup"><span data-stu-id="17d8b-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="17d8b-144">Viene chiamato l'helper Html.ValidationMessage() accanto a ogni campo di form HTML.</span><span class="sxs-lookup"><span data-stu-id="17d8b-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="17d8b-145">Questo supporto consente di visualizzare un messaggio di errore a destra, accanto a un campo del form.</span><span class="sxs-lookup"><span data-stu-id="17d8b-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="17d8b-146">Nel caso listato 2, l'helper Html.ValidationMessage() Visualizza un asterisco, quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="17d8b-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="17d8b-147">La pagina nella figura 3 illustra i messaggi di errore viene eseguito il rendering per gli helper di convalida quando il form viene inviato con i campi mancanti e valori non validi.</span><span class="sxs-lookup"><span data-stu-id="17d8b-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="17d8b-148">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="17d8b-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="17d8b-149">**Figura 03**: crea la vista inviato con problemi ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="17d8b-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="17d8b-150">Si noti che l'aspetto del codice HTML di input campi vengono modificati anche quando si verifica un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="17d8b-151">I renderer di helper Html.TextBox() una *classe = "errore di convalida input"* attributo quando si verifica un errore di convalida associato alla proprietà viene eseguito il rendering dall'helper Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="17d8b-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="17d8b-152">Esistono tre le classi foglio di stile utilizzate per controllare l'aspetto degli errori di convalida:</span><span class="sxs-lookup"><span data-stu-id="17d8b-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="17d8b-153">input-errore di convalida - applicato per il &lt;input&gt; sottoposto a rendering dal Html.TextBox() helper tag.</span><span class="sxs-lookup"><span data-stu-id="17d8b-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="17d8b-154">campo-errore di convalida - applicato per il &lt;span&gt; tag viene eseguito il rendering dall'helper Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="17d8b-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="17d8b-155">Riepilogo-errori di convalida - applicato per il &lt;ul&gt; tag viene eseguito il rendering dall'helper Html.ValidationSumamry().</span><span class="sxs-lookup"><span data-stu-id="17d8b-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="17d8b-156">È possibile modificare queste classi di foglio di stile CSS e quindi modificare l'aspetto di errori di convalida, modificando il file CSS che si trova nella cartella del contenuto.</span><span class="sxs-lookup"><span data-stu-id="17d8b-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="17d8b-157">La classe HtmlHelper include le proprietà statiche di sola lettura per il recupero della convalida i nomi relativi a CSS classi.</span><span class="sxs-lookup"><span data-stu-id="17d8b-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="17d8b-158">Queste proprietà statiche sono denominate ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="17d8b-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="17d8b-159">Prebinding Postbinding convalida e convalida</span><span class="sxs-lookup"><span data-stu-id="17d8b-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="17d8b-160">Se si invia il form HTML per la creazione di un prodotto e immettere un valore non valido per il campo price e nessun valore per il campo UnitsInStock, ricevere i messaggi di convalida visualizzati nella figura 4.</span><span class="sxs-lookup"><span data-stu-id="17d8b-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="17d8b-161">Questi messaggi di errore di convalida da dove provengono?</span><span class="sxs-lookup"><span data-stu-id="17d8b-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="17d8b-162">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17d8b-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="17d8b-163">**Figura 04**: gli errori di convalida Prebinding ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="17d8b-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="17d8b-164">Esistono effettivamente due tipi di messaggi di errore di convalida - quelli generati prima che i campi di form HTML sono associati a una classe e quelli generati dopo che i campi del modulo sono associati alla classe.</span><span class="sxs-lookup"><span data-stu-id="17d8b-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="17d8b-165">In altre parole, esistono prebinding gli errori di convalida e postbinding gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="17d8b-166">L'azione create () esposto dal controller di prodotto nel listato 1 accetta un'istanza della classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="17d8b-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="17d8b-167">La firma del metodo di creazione simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="17d8b-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="17d8b-168">I valori dei campi HTML dal modulo di creazione vengono associati alla classe productToCreate da un componente chiamato un raccoglitore di modelli.</span><span class="sxs-lookup"><span data-stu-id="17d8b-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="17d8b-169">Il raccoglitore dei modelli predefiniti aggiunge un messaggio di errore per lo stato del modello automaticamente quando Impossibile associare un campo del form a una proprietà di form.</span><span class="sxs-lookup"><span data-stu-id="17d8b-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="17d8b-170">Lo strumento di associazione del modello predefinito non è possibile associare la stringa "apple" alla proprietà prezzo della classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="17d8b-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="17d8b-171">È possibile assegnare una stringa a una proprietà decimale.</span><span class="sxs-lookup"><span data-stu-id="17d8b-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="17d8b-172">Pertanto, lo strumento di associazione del modello aggiunge un errore per lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="17d8b-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="17d8b-173">Lo strumento di associazione del modello predefinito non è possibile inoltre assegnare un valore null a una proprietà che non accetta valori null.</span><span class="sxs-lookup"><span data-stu-id="17d8b-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="17d8b-174">In particolare, il gestore di associazione del modello non è possibile assegnare un valore null alla proprietà UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="17d8b-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="17d8b-175">Ancora una volta, lo strumento individuerebbe rinuncia e aggiunge un messaggio di errore allo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="17d8b-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="17d8b-176">Se si vuole personalizzare l'aspetto di questi prebinding i messaggi di errore è necessario creare stringhe di risorse per questi messaggi.</span><span class="sxs-lookup"><span data-stu-id="17d8b-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="17d8b-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="17d8b-177">Summary</span></span>

<span data-ttu-id="17d8b-178">L'obiettivo di questa esercitazione è descrivere i meccanismi di base di convalida nel framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="17d8b-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="17d8b-179">Si è appreso come usare lo stato del modello e gli helper HTML di convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="17d8b-180">Abbiamo parlato anche la distinzione tra prebinding e postbinding convalida.</span><span class="sxs-lookup"><span data-stu-id="17d8b-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="17d8b-181">In altre esercitazioni si parlerà diverse strategie per spostare il codice di convalida del controller e nelle classi di modelli.</span><span class="sxs-lookup"><span data-stu-id="17d8b-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="17d8b-182">[Precedente](displaying-a-table-of-database-data-cs.md)
> [Successivo](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="17d8b-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
