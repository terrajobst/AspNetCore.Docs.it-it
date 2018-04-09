---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Esegue la convalida semplice (c#) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione, Stephen Walther introduce di stato del modello e l'helper HTML di convalida...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fc1dcc6935841382215f67a519cd241ac68931a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="6334e-104">Esegue la convalida semplice (c#)</span><span class="sxs-lookup"><span data-stu-id="6334e-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="6334e-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6334e-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="6334e-106">Informazioni su come eseguire la convalida in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6334e-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="6334e-107">In questa esercitazione, Stephen Walther introduce di stato del modello e gli helper di convalida HTML.</span><span class="sxs-lookup"><span data-stu-id="6334e-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="6334e-108">L'obiettivo di questa esercitazione è illustrare come è possibile eseguire la convalida all'interno di un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6334e-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="6334e-109">Ad esempio, come impedisce l'invio di un modulo che non contiene un valore per un campo obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="6334e-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="6334e-110">Informazioni su come utilizzare lo stato del modello e gli helper HTML di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="6334e-111">Informazioni sullo stato di modello</span><span class="sxs-lookup"><span data-stu-id="6334e-111">Understanding Model State</span></span>

<span data-ttu-id="6334e-112">Utilizzare lo stato del modello - o in modo più accurato, il dizionario di stato del modello - per rappresentare gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="6334e-113">Ad esempio, l'azione del metodo di creazione nel listato 1 convalida le proprietà di una classe di prodotto prima di aggiungere la classe di prodotto in un database.</span><span class="sxs-lookup"><span data-stu-id="6334e-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="6334e-114">Sono non è consigliato aggiungere la logica di convalida o di database a un controller.</span><span class="sxs-lookup"><span data-stu-id="6334e-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="6334e-115">Un controller deve contenere solo la logica correlata al controllo di flusso dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6334e-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="6334e-116">Si sta eseguendo un collegamento per semplificare le operazioni.</span><span class="sxs-lookup"><span data-stu-id="6334e-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="6334e-117">**Elenco 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="6334e-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="6334e-118">Nel listato 1, vengono convalidate le proprietà UnitsInStock, la descrizione e nome della classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="6334e-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="6334e-119">Se una di queste proprietà ha esito negativo un test di convalida viene aggiunto un errore al dizionario di stato del modello (rappresentato dalla proprietà della classe Controller ModelState).</span><span class="sxs-lookup"><span data-stu-id="6334e-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="6334e-120">Se sono presenti errori di stato del modello la proprietà ModelState.IsValid restituisce false.</span><span class="sxs-lookup"><span data-stu-id="6334e-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="6334e-121">In tal caso, verrà nuovamente visualizzato il form HTML per la creazione di un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="6334e-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="6334e-122">In caso contrario, se non sono presenti errori di convalida, il nuovo prodotto viene aggiunto al database.</span><span class="sxs-lookup"><span data-stu-id="6334e-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="6334e-123">Utilizzando l'helper di convalida</span><span class="sxs-lookup"><span data-stu-id="6334e-123">Using the Validation Helpers</span></span>

<span data-ttu-id="6334e-124">Il framework di MVC ASP.NET include due helper di convalida: l'helper Html.ValidationMessage() e il supporto Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="6334e-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="6334e-125">Utilizzare questi due helper in una vista per visualizzare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="6334e-126">Gli helper Html.ValidationMessage() e Html.ValidationSummary() vengono utilizzati nelle viste di creare e modificare che vengono generate automaticamente dallo scaffolding di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6334e-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="6334e-127">Seguire questi passaggi per generare la visualizzazione di creazione:</span><span class="sxs-lookup"><span data-stu-id="6334e-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="6334e-128">L'azione del metodo di creazione del controller di prodotto e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="6334e-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="6334e-129">Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata** (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="6334e-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="6334e-130">Dal **visualizzare dati classe** elenco a discesa, selezionare la classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="6334e-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="6334e-131">Dal **visualizzare il contenuto** crea seleziona nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="6334e-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="6334e-132">Fare clic sul pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="6334e-132">Click the **Add** button.</span></span>


<span data-ttu-id="6334e-133">Assicurarsi che si compila l'applicazione prima di aggiungere una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="6334e-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="6334e-134">In caso contrario, l'elenco delle classi non verrà visualizzata nella **visualizzare dati classe** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="6334e-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="6334e-135">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6334e-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="6334e-136">**Figura 01**: aggiunta di una vista ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6334e-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="6334e-137">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6334e-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="6334e-138">**Figura 02**: creazione di una visualizzazione fortemente tipizzata ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="6334e-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="6334e-139">Dopo aver completato questi passaggi, si ottiene la visualizzazione di creazione nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="6334e-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="6334e-140">**Il listato 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="6334e-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="6334e-141">Nel listato 2, l'helper Html.ValidationSummary() viene chiamato immediatamente sopra il form HTML.</span><span class="sxs-lookup"><span data-stu-id="6334e-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="6334e-142">Questo supporto viene utilizzato per visualizzare un elenco di messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="6334e-143">L'helper Html.ValidationSummary() esegue il rendering gli errori in un elenco puntato.</span><span class="sxs-lookup"><span data-stu-id="6334e-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="6334e-144">L'helper Html.ValidationMessage() viene chiamato accanto a ognuno dei campi del form HTML.</span><span class="sxs-lookup"><span data-stu-id="6334e-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="6334e-145">Questo supporto viene utilizzato per visualizzare un messaggio di errore destra accanto a un campo del form.</span><span class="sxs-lookup"><span data-stu-id="6334e-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="6334e-146">Nel caso di listato 2, l'helper Html.ValidationMessage() viene visualizzato un asterisco quando si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="6334e-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="6334e-147">La pagina nella figura 3 illustra i messaggi di errore visualizzati per gli helper di convalida quando il form viene inviato con i campi mancanti e valori non validi.</span><span class="sxs-lookup"><span data-stu-id="6334e-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="6334e-148">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6334e-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="6334e-149">**Figura 03**: crea la vista inviato con i problemi ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6334e-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="6334e-150">Si noti che l'aspetto del codice HTML input campi vengono modificati anche quando si verifica un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="6334e-151">I renderer di helper Html.TextBox() un *classe = "errore di convalida input"* attributo quando si verifica un errore di convalida associati alla proprietà di cui è eseguito il rendering dall'helper della Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="6334e-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="6334e-152">Esistono tre le classi foglio di stile utilizzate per controllare l'aspetto di errori di convalida:</span><span class="sxs-lookup"><span data-stu-id="6334e-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="6334e-153">Errore di convalida input - applicato per il &lt;input&gt; tag sottoposto a rendering dal supporto Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="6334e-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="6334e-154">Errore - convalida campo collegato il &lt;span&gt; tag dopo il rendering dall'helper della Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="6334e-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="6334e-155">errori di riepilogo convalida - applicato per il &lt;ul&gt; tag dopo il rendering dall'helper della Html.ValidationSumamry().</span><span class="sxs-lookup"><span data-stu-id="6334e-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="6334e-156">È possibile modificare queste classi di foglio di stile CSS e quindi modificare l'aspetto di errori di convalida, modificando il file Site.css che si trova nella cartella del contenuto.</span><span class="sxs-lookup"><span data-stu-id="6334e-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="6334e-157">La classe HtmlHelper include le proprietà statiche di sola lettura per il recupero dei nomi della convalida correlati a CSS classi.</span><span class="sxs-lookup"><span data-stu-id="6334e-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="6334e-158">Queste proprietà statiche sono denominate ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="6334e-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="6334e-159">Prebinding convalida e convalida Postbinding</span><span class="sxs-lookup"><span data-stu-id="6334e-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="6334e-160">Se si invia il form HTML per la creazione di un prodotto e immettere un valore non valido per il campo Prezzo e nessun valore del campo scorte, si otterrà i messaggi di convalida visualizzati nella figura 4.</span><span class="sxs-lookup"><span data-stu-id="6334e-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="6334e-161">Da dove provengono questi messaggi di errore di convalida?</span><span class="sxs-lookup"><span data-stu-id="6334e-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="6334e-162">[![La finestra di dialogo Nuovo progetto](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6334e-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="6334e-163">**Figura 04**: errori di convalida Prebinding ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="6334e-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="6334e-164">Esistono effettivamente due tipi di messaggi di errore - convalida quelli generati prima i campi di form HTML sono associati a una classe e quelli generato dopo che i campi del modulo sono associati alla classe.</span><span class="sxs-lookup"><span data-stu-id="6334e-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="6334e-165">In altre parole, sono presenti errori di convalida prebinding e postbinding gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="6334e-166">L'azione del metodo di creazione esposto dal controller di prodotto nel listato 1 accetta un'istanza della classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="6334e-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="6334e-167">La firma del metodo di creazione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6334e-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="6334e-168">I valori dei campi nel form HTML il modulo Crea associati alla classe productToCreate da un elemento è stato chiamato un raccoglitore di modelli.</span><span class="sxs-lookup"><span data-stu-id="6334e-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="6334e-169">Lo strumento di associazione predefinito aggiunge un messaggio di errore per lo stato del modello automaticamente quando Impossibile associare un campo modulo a una proprietà del modulo.</span><span class="sxs-lookup"><span data-stu-id="6334e-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="6334e-170">Lo strumento di associazione predefinito non è possibile associare la proprietà di prezzo della classe di prodotto la stringa "apple".</span><span class="sxs-lookup"><span data-stu-id="6334e-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="6334e-171">È possibile assegnare una stringa a una proprietà decimale.</span><span class="sxs-lookup"><span data-stu-id="6334e-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="6334e-172">Pertanto, lo strumento di associazione aggiunge un errore per lo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="6334e-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="6334e-173">Lo strumento di associazione predefinito non è possibile inoltre assegnare un valore null a una proprietà che non accetta valori null.</span><span class="sxs-lookup"><span data-stu-id="6334e-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="6334e-174">In particolare, il gestore di associazione del modello non è possibile assegnare un valore null alla proprietà UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="6334e-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="6334e-175">In questo caso, il gestore di associazione del modello rinunciare e aggiunge un messaggio di errore allo stato del modello.</span><span class="sxs-lookup"><span data-stu-id="6334e-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="6334e-176">Se si desidera personalizzare l'aspetto di questi messaggi di errore prebinding è necessario creare stringhe di risorse per questi messaggi.</span><span class="sxs-lookup"><span data-stu-id="6334e-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="6334e-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6334e-177">Summary</span></span>

<span data-ttu-id="6334e-178">L'obiettivo di questa esercitazione è stata per descrivere il funzionamento di base di convalida nel framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6334e-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="6334e-179">È stato illustrato come utilizzare lo stato del modello e gli helper HTML di convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="6334e-180">Abbiamo parlato anche la distinzione tra prebinding e postbinding convalida.</span><span class="sxs-lookup"><span data-stu-id="6334e-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="6334e-181">In altre esercitazioni si parlerà diverse strategie per spostare il codice di convalida, i controller e nelle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="6334e-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6334e-182">[Precedente](displaying-a-table-of-database-data-cs.md)
> [Successivo](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6334e-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
